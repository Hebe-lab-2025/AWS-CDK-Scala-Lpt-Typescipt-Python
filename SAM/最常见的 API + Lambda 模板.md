## 1) 🔥 “考点级”逐行拆 `template.yaml`（用一份标准 SAM 模板举例）

> 你可以把你自己的 `template.yaml` 贴出来，我能按你那份**一行不漏**逐行标注。
> 先用这份**最常见的 API + Lambda** 模板做“考点版逐行拆”。

```yaml
AWSTemplateFormatVersion: '2010-09-09'     # CloudFormation 模板版本声明（几乎固定写法）
Transform: AWS::Serverless-2016-10-31      # ✅考点：声明这是 SAM 模板（开启 SAM 语法糖）

Description: Simple SAM API + Lambda       # 模板说明（不影响资源）

Globals:                                  # ✅考点：全局默认配置（减少重复）
  Function:
    Timeout: 10                            # 所有 Lambda 默认超时 10s（可被单个函数覆盖）
    MemorySize: 128                        # 默认内存 128MB
    Runtime: python3.12                    # 默认运行时（可覆盖）
    Tracing: Active                        # ✅考点：开启 X-Ray（需要权限/实际AWS环境更相关）
  Api:
    TracingEnabled: true                   # API Gateway 也启用 tracing（AWS 部署更相关）

Resources:                                 # CloudFormation 资源入口
  HelloFunction:                           # 逻辑名（模板内部引用用）
    Type: AWS::Serverless::Function        # ✅考点：SAM 的 Function 资源类型
    Properties:
      FunctionName: hello-sam              # （可选）真实 Lambda 名称；不写就自动生成
      CodeUri: src/                        # ✅考点：代码位置（相对路径到函数代码目录/zip）
      Handler: app.lambda_handler          # ✅考点：入口：<文件名>.<函数名>
      Policies:                            # ✅考点：快速给 IAM 权限（SAM Policy Templates 或 AWS managed）
        - AWSLambdaBasicExecutionRole      # 允许写 CloudWatch Logs（最常见）
      Environment:                         # 环境变量（本地也会注入）
        Variables:
          STAGE: dev
      Events:                              # ✅考点：事件源（SAM 自动帮你建触发器）
        ApiEvent:
          Type: Api                        # ✅考点：API Gateway 触发
          Properties:
            Path: /hello                   # 路由路径
            Method: GET                    # HTTP 方法

  HelloApi:                                # （可选）显式 API 资源；不写也可由 Events 隐式生成
    Type: AWS::Serverless::Api             # ✅考点：SAM API 资源类型
    Properties:
      StageName: dev                       # API stage
      EndpointConfiguration: REGIONAL      # 常见：REGIONAL（也可 EDGE/PRIVATE）
      Cors:                                # ✅考点：CORS 配置（前端调用常遇到）
        AllowMethods: "'GET,OPTIONS'"
        AllowHeaders: "'Content-Type,Authorization'"
        AllowOrigin: "'*'"

Outputs:                                   # ✅考点：输出（部署后最常用来拿 URL / ARN）
  ApiUrl:
    Description: API endpoint URL
    Value: !Sub "https://${HelloApi}.execute-api.${AWS::Region}.amazonaws.com/dev/hello"
  HelloFunctionArn:
    Description: Lambda ARN
    Value: !GetAtt HelloFunction.Arn
```

### ✅ 高频考点速记（面试/考试常问）

* `Transform: AWS::Serverless-2016-10-31`：**没有它就不是 SAM**
* `CodeUri + Handler + Runtime`：三件套，缺一不可（Java/Node 写法不同）
* `Events: Type: Api`：SAM 会自动生成 API Gateway 路由（也可显式建 `AWS::Serverless::Api`）
* `Globals`：减少重复配置，但**单个资源可覆盖**
* `Outputs`：部署后拿 URL/ARN 的最常用出口

---

## 2) 🧪 用 `sam local invoke` 本地跑一次 Lambda（一步到位）

### A. 准备最小函数代码（例：`src/app.py`）

```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "hello from sam local"
    }
```

### B. 本地构建（一定要做）

```bash
sam build
```

**为什么要 build？**
SAM 会把依赖、路径、编译产物放到 `.aws-sam/build/`，`sam local` 默认跑这里。

### C. 本地 invoke（两种方式）

**方式 1：直接 invoke（不走 HTTP）**

```bash
sam local invoke HelloFunction
```

**方式 2：带 event JSON**

```bash
echo '{"path":"/hello","httpMethod":"GET"}' > event.json
sam local invoke HelloFunction -e event.json
```

### D. 常见报错（你基本会遇到）

* **Docker 没开 / 找不到 Docker**：`sam local` 依赖 Docker 跑 runtime 容器
* **Handler 写错**：`Handler: app.lambda_handler` 但文件不是 `app.py` 或函数名不对
* **CodeUri 路径不对**：`src/` 里没代码，或者 template 相对路径搞错

---

## 3) 🧠 `sam init → sam deploy → API 调用` 全流程图（一张图看懂）

```text
[1] sam init
    |
    |-- 生成项目骨架: template.yaml + src/ + 依赖文件
    v
[2] sam build
    |
    |-- 打包/编译/依赖收集
    |-- 输出到: .aws-sam/build/
    v
[3] sam deploy (guided)
    |
    |-- (a) 打包上传到 S3 (deployment bucket)
    |-- (b) CloudFormation 创建资源
    |       - Lambda
    |       - IAM Role/Policy
    |       - API Gateway (routes/stage)
    |
    |-- 输出 Outputs: API URL / Function ARN
    v
[4] 调用 API
    |
    |-- curl / Postman / 前端 axios fetch
    v
[5] 运行链路（线上）
    API Gateway -> Lambda Invoke -> CloudWatch Logs
```

---

如果你把你 lab 的 `template.yaml`（或你项目的那份）贴出来，我会按你那份做**逐行“考点级注释”**，并且把你 `sam local invoke` 需要的 `event.json` 也一起配好（你只要复制命令就能跑）。
