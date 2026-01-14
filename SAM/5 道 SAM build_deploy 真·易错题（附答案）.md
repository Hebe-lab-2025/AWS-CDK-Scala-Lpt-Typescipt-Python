## 🔍 先说清楚：我现在看不到你本机的 `.aws-sam/build/template.yaml`

所以没法“对照你那份文件逐行解释”。
但我可以做到两件事（你不用等）：

1. **给你一份 `.aws-sam/build/template.yaml` 的典型形态**（SAM build 后最常见的结构），按“逐行考点”解释：你拿它去对照你文件，基本每行都能对上。
2. 给你一张**“怎么对照”的检查清单**：你自己打开 build 产物，就能快速定位每处差异意味着什么。

> 如果你愿意贴出 `.aws-sam/build/template.yaml`（或关键片段：Transform/Resources/Outputs），我可以把下面的“模板级讲解”升级成**逐行逐资源对你文件的精确标注**。

---

## 🔍 `.aws-sam/build/template.yaml` 典型长什么样（并逐行解释）

> 这是最常见：**SAM Transform + Function + Api Event**
> build 后主要变化：`CodeUri`、一些隐式资源展开、路径指向 `.aws-sam/build/...`

```yaml
AWSTemplateFormatVersion: '2010-09-09'          # CFN 版本声明
Transform: AWS::Serverless-2016-10-31           # ✅仍然是 SAM（build 不会把 Transform “变没”）
Description: Built template                      # build 后常被替换/保留（不重要）

Globals:                                         # ✅Globals 仍在（很多 build 不会动它）
  Function:
    Runtime: python3.12
    Timeout: 10

Resources:
  CoursesApi:                                    # 你自定义的 API（如果原来有）
    Type: AWS::Serverless::Api
    Properties:
      StageName: dev
      Cors:
        AllowMethods: "'GET,OPTIONS'"
        AllowHeaders: "'Content-Type,Authorization'"
        AllowOrigin: "'*'"

  GetCoursesFunction:                            # 你自定义的 Lambda 逻辑名
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: GetCoursesFunction/               # ✅关键变化：build 之后 CodeUri 指向 build 目录里的产物
      Handler: app.lambda_handler                # ✅不变：入口仍是同一个 handler
      Runtime: python3.12                        # 可能来自 Globals，也可能这里显式写了
      Environment:
        Variables:
          COURSES_TABLE: Courses
      Policies:
        - AWSLambdaBasicExecutionRole
        - Statement:
            - Effect: Allow
              Action:
                - dynamodb:GetItem
                - dynamodb:Query
                - dynamodb:Scan
              Resource:
                - arn:aws:dynamodb:us-west-2:123456789012:table/Courses
      Events:
        GetCoursesApi:
          Type: Api
          Properties:
            RestApiId: !Ref CoursesApi           # ✅Ref 返回 API 的 RestApiId
            Path: /courses
            Method: GET

Outputs:
  CoursesApiUrl:
    Description: GET /courses endpoint
    Value: !Sub "https://${CoursesApi}.execute-api.${AWS::Region}.amazonaws.com/dev/courses"
```

### ✅ 你对照你那份 build 文件时，重点盯这几类“变化点”

* **`CodeUri` 变化**：

  * 源模板可能是 `src/get_courses/`
  * build 后通常变成 `GetCoursesFunction/` 或类似目录名（位于 `.aws-sam/build/` 里）
* **隐式资源可能被展开/补全**（取决于你用到什么）

  * 例如你没写 `AWS::Lambda::Permission`，部署时 CloudFormation 会需要它（SAM Transform 可能生成）
* **依赖打包结果**：

  * Python：requirements 可能被装进 `.aws-sam/build/<Func>/`
  * Node：node_modules 可能被装进去
  * Java：可能指向打包 jar

---

## 🔍 “逐行对照”自检清单（你照着看，能立刻定位问题）

在 `.aws-sam/build/template.yaml` 里按顺序搜这些关键词：

1. `Transform:`

   * ✅必须存在：没有就不是 SAM 模板了（除非你用 `sam package` 产物走纯 CFN，但 build 一般仍保留）

2. `Resources:` 下每个 `AWS::Serverless::Function`

   * 看 `CodeUri:` 是否指向 **build 子目录**
   * 看 `Handler:` 是否和你的代码文件一致
   * 看 `Runtime:` 是否符合你期望（有时 Globals 覆盖/继承导致“你以为是 X 实际是 Y”）

3. `Events:` → `Type: Api` 或 `HttpApi`

   * `Path/Method` 是否和你想暴露的路由一致
   * `RestApiId` 是否指向你那一个 API（`!Ref`）

4. `Outputs:`

   * 你是否在用 `${ApiLogicalId}` 拼 URL（这是 `RestApiId` 不是 URL）

---

## 🧪 5 道 SAM build / deploy 真·易错题（附答案）

### 1) 你改了代码，直接 `sam deploy`，结果线上没变。为什么？

**答案要点：**
你没重新 `sam build`（或没用 `--build`），deploy 用的还是旧的 `.aws-sam/build` 产物。

---

### 2) `sam build` 成功，但 `sam local invoke` 报 “Unable to import module …”

**答案要点：**
`CodeUri/Handler/目录结构` 对不上：

* `CodeUri` 指到错误目录
* `Handler` 文件名或函数名不一致
* Python 包引用缺 `__init__.py`（当你用包路径 Handler 时）

---

### 3) `sam deploy` 报 S3 上传/权限错误，为什么？

**答案要点：**
deploy 需要一个 **deployment bucket**（用于上传打包产物）；常见原因：

* 当前 AWS profile/role 没有 `s3:PutObject`
* bucket 不存在或 region 不匹配
* 企业环境有 SCP/权限边界限制

---

### 4) 本地 `sam local start-api` 能跑，部署后 API 访问 403/500

**答案要点：**
本地不会暴露真实 IAM/资源权限问题；线上常见：

* Lambda role 没有 DynamoDB 权限（GetItem/Query/Scan）
* 表名/环境变量错了（dev/prod 混了）
* CORS 配置不对导致前端被浏览器拦截（看起来像“请求失败”）

---

### 5) 同一个模板，多次 deploy，有时资源被替换（URL 变了/函数名变了）

**答案要点：**
你没固定物理名（例如没写 `FunctionName`），或者改了会触发替换的属性；
替换会导致新资源生成，旧资源删除（RestApiId 变化就会导致 URL 变化）。

---

## 🧠 执行图：SAM → CloudFormation → AWS 资源（从命令到资源落地）

```text
Developer CLI
   |
   | 1) sam build
   |    - 解析 template.yaml
   |    - 安装依赖/编译
   |    - 产出 .aws-sam/build/* + build/template.yaml
   v
Build Artifacts (.aws-sam/build)
   |
   | 2) sam deploy
   |    - 打包产物（zip/jar/etc）
   |    - 上传到 S3 (deployment bucket)
   |    - 生成/提交 CloudFormation ChangeSet
   v
CloudFormation Stack
   |
   | 3) SAM Transform (AWS::Serverless-2016-10-31)
   |    - 把 Serverless 资源“展开”为底层 AWS 资源
   |    - 例：Api routes / Lambda permission / IAM role 等
   v
AWS Resources Created/Updated
   |
   |-- API Gateway (REST API / HTTP API)
   |-- Lambda Function + LogGroup
   |-- IAM Role/Policy (Lambda execution role)
   |-- (Optional) DynamoDB / S3 / EventBridge / etc.
   |
   v
Runtime Call Path
Client -> API Gateway -> Lambda -> DynamoDB
                   -> CloudWatch Logs
```

---

### 你要我“对照你当前 build/template.yaml 逐行解释”的最快方式

把下面任意一种内容粘出来就行（不需要全贴）：

* `.aws-sam/build/template.yaml` 的 **Transform + Resources（Function/Api）+ Outputs** 段
* 或者直接贴你 `GetCoursesFunction` 那一段资源定义

我就能把你文件里的**每个 `CodeUri/Handler/Ref/Sub/GetAtt` 的真实含义**一行一行标出来。
