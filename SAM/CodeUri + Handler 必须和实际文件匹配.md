下面我按 **“这份 SAM 模板做了什么” + “会踩的坑/为什么你刚才只看到 DynamoDB” + “怎么改到可用”** 三块给你拆。

---

## 1) 这份模板现在会创建哪些资源？

### ✅ 全局设置（Globals）

```yaml
Globals:
  Function:
    Timeout: 3
    Tracing: Active
    Runtime: nodejs18.x
```

* 所有 Lambda 默认：超时 3 秒、X-Ray 开、Node 18
* **注意**：你前面 lab 说要 **关闭 X-Ray**，这里却是 `Active`（不影响部署，但和 lab 要求不一致）。

---

### ✅ DynamoDB 表

```yaml
CourseTable:
  Type: AWS::Serverless::SimpleTable
  Properties:
    TableName: Courses
    PrimaryKey:
      Name: ID
      Type: Number
```

* 创建一个 DynamoDB 表：`Courses`
* 主键 ID（Number）

---

### ✅ 4 个 Lambda（CRUD）

每个函数都指向同一个代码目录：

```yaml
CodeUri: functions/
```

并用不同 handler：

* `readCourses.readCourses`
* `createCourses.createCourses`
* `updateCourses.updateCourses`
* `deleteCourses.deleteCourses`

并且都设置环境变量：

```yaml
COURSE_TABLE: !Ref CourseTable
```

---

## 2) 你这份模板最可能导致部署失败/行为异常的点（重点）

### 🚨 坑 1：你显式指定了一个 **“必须存在的 IAM Role”**

```yaml
Role: !Sub arn:aws:iam::${AWS::AccountId}:role/ClabLambdaRole
```

这表示：**CloudFormation 不会帮你创建 role**，而是要求账号里已经有：

* Role 名字：`ClabLambdaRole`

如果这个 role 不存在，部署会报：

* `Role cannot be assumed` / `role does not exist` 之类错误。

✅ 解决方式（两选一）：

1. **最推荐**：让 SAM 自动创建执行角色（删掉 `Role:` 行），并在模板里用 `Policies` 授权 DynamoDB
2. 如果 lab 要你用固定 role：确保 `ClabLambdaRole` 已经存在且有 DynamoDB 权限

---

### 🚨 坑 2：你创建了 Lambda，但**没有任何触发器**（API Gateway/Event）

你模板里只有 `AWS::Serverless::Function`，但是没有：

* `Events:`（API / HTTP / Schedule / SQS 等）
* `AWS::Serverless::Api`

所以就算部署成功：

* 你**没法通过 URL 调用**
* 只能手动 invoke 或自己再配置触发器

✅ 这也是你之前说 “浏览器访问 hello” 那种效果现在不会出现的原因。

---

### 🚨 坑 3：CodeUri + Handler 必须和实际文件匹配

你写的是：

```yaml
CodeUri: functions/
Handler: readCourses.readCourses
```

这要求你的目录必须长这样（Node.js Zip 模式）：

```
functions/
  readCourses.js        # 导出 readCourses 方法
  createCourses.js
  updateCourses.js
  deleteCourses.js
```

并且每个文件里必须有：

```js
exports.readCourses = async (event) => { ... }
```

否则部署能过，但运行会报：

* `Runtime.ImportModuleError`
* `Handler 'readCourses' missing` 等。

---

## 3) 让它“按 CRUD API 跑起来”的最小改法（直接给你一版）

### ✅ 改法 A（最省事）：删掉固定 Role，让 SAM 自动建 + 加 API 事件

你可以把每个函数加上 `Events`（API Gateway）：

```yaml
GetCourses:
  Type: AWS::Serverless::Function
  Properties:
    CodeUri: functions/
    Handler: readCourses.readCourses
    Environment:
      Variables:
        COURSE_TABLE: !Ref CourseTable
    Policies:
      - DynamoDBReadPolicy:
          TableName: !Ref CourseTable
    Events:
      Api:
        Type: Api
        Properties:
          Path: /courses
          Method: GET
```

Insert/Update/Delete 类似（写权限用 `DynamoDBCrudPolicy` 更简单）。

> 这样部署完你会在 Outputs 里拿到 API endpoint，然后浏览器/Postman 就能打通。

---

## 4) 你现在这份模板里，我建议你立刻改的 2 个地方

1. 如果 lab 要 “Disable X-Ray”，改：

```yaml
Tracing: PassThrough   # 或者直接删掉 Tracing
```

2. 如果你不确定 ClabLambdaRole 是否存在：先删掉 `Role:`，改用 `Policies:`（SAM 自动创建 role）

---

如果你把你现在的 `functions/` 目录结构（`ls -R functions`）贴出来，我可以直接告诉你：

* Handler 写得对不对
* 文件导出名对不对
* 以及帮你补齐完整的 CRUD API（含 Outputs 的 API URL）。
