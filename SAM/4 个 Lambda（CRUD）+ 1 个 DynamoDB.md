这段话在说：你已经有 **4 个 Lambda（CRUD）+ 1 个 DynamoDB 表**了，但现在**外部还没法用 HTTP URL 调用它们**；所以要加 **API Gateway**，把不同的 HTTP 请求路由到对应的 Lambda。

---

## 这段话逐句是什么意思？

### 1) “We’ve already created Lambda functions … CRUD on DynamoDB.”

你已经写好了：

* GetCourses / InsertCourse / UpdateCourse / DeleteCourse
  它们内部用 DynamoDB SDK 对 `Courses` 表做增删改查。

---

### 2) “Now, we need to access those Lambda functions through API endpoints.”

你现在缺的是 **endpoint（URL）**，也就是：

* `GET https://.../courses`
* `POST https://.../courses`
* `PUT https://.../courses/{id}`
* `DELETE https://.../courses/{id}`

没有 API Gateway，你只能：

* 控制台点 Invoke
* 或 `aws lambda invoke`
  但不能像正常后端一样通过浏览器/Postman/前端调用。

---

### 3) “AWS::Serverless::Api … create a REST API.”

这是 **SAM 的资源类型**：

```yaml
Type: AWS::Serverless::Api
```

它是 CloudFormation 的“更省事写法”（SAM 语法糖），用来创建 **API Gateway REST API**（不是 HTTP API）。

---

### 4) “An OpenAPI document defines the configurations of the API.”

**OpenAPI 文档**就是一个规范文件（YAML/JSON），描述：

* 有哪些 path（路由）
* 每个 path 支持哪些 method（GET/POST/PUT/DELETE）
* 每个 method 后面接哪个 Lambda（integration）

简单说：

> OpenAPI = “API 的说明书 + 路由表 + 绑定 Lambda 的配置文件”。

---

### 5) “create an API Gateway to route HTTP(S) requests to Lambda”

就是把：

* `GET /courses` → GetCourses Lambda
* `POST /courses` → InsertCourse Lambda
* `PUT /courses/{id}` → UpdateCourse Lambda
* `DELETE /courses/{id}` → DeleteCourse Lambda

绑定起来。

---

## “provisioned infrastructure is shown below” 是啥意思？

这里的 **provisioned infrastructure** 指的是：

> 通过 SAM/CloudFormation **实际被创建出来的云资源拓扑**（部署后的架构图）

通常长这样（ASCII 图）：

```
Client (Browser/Postman)
        |
        |  HTTPS request
        v
API Gateway (REST API)
  |     |      |      |
  v     v      v      v
Get   Insert  Update  Delete   (Lambda)
  \      |       |      /
         v
     DynamoDB Table (Courses)
```

---

## 你下一步在 template.yaml 里会加什么（直觉版）

你会新增一段：

* 一个 `AWS::Serverless::Api`
* 然后在每个 Function 里加 `Events: Api`（或在 OpenAPI 里写 integration）

两种常见做法：

1. **SAM Events 写法**（更简单）
2. **OpenAPI 写法**（更“正规”，适合复杂配置）

你这段话明确说：**用 OpenAPI 文档配置**，所以更可能是第 2 种。

---

如果你把“下面那张 provisioned infrastructure 图”或 OpenAPI 片段贴出来，我可以直接帮你把四个路由和四个 Lambda **一一对齐**，并告诉你最终会得到哪些 URL（Invoke endpoint）。
