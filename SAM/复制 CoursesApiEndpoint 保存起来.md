这段话整体方向是对的：**SAM 更抽象、CloudFormation 更显式**。但里面有几处需要你记成更“严谨的考试版”。我按要点改写+补充。

---

## 1) “复制 CoursesApiEndpoint 保存起来”是什么意思？

你部署输出里这条：

* `CoursesApiEndpoint = https://...execute-api...amazonaws.com/Dev/`

它就是你后面做所有 API 调用的 **base URL**。
后续你会拼成：

* `GET  {base}/course`
* `POST {base}/course`
* `PUT  {base}/course/{id}`
* `DELETE {base}/course/{id}`

而 CloudFormation 控制台的 **Outputs tab** 只是：部署后再去查这些值的地方。

---

## 2) SAM vs CloudFormation：API Gateway 的关键差异（更准确版）

### 差异 A：API 定义方式（OpenAPI vs CFN Properties）

你的文档说：

> SAM 需要 OpenAPI；CloudFormation 在 Properties 里定义 paths

✅ 更准确讲法是：

* **API Gateway REST API 的“路由/方法/集成”配置，本质上还是 OpenAPI（Swagger 2.0）** 最直接

* **SAM 常用两种方式**：

  1. `DefinitionBody / DefinitionUri` 引入 OpenAPI（你现在用的）
  2. 在 Lambda 里用 `Events: Api`（SAM 自动生成 OpenAPI/方法/集成）

* **CloudFormation（AWS::ApiGateway::*）** 方式通常是显式创建一堆资源：

  * `AWS::ApiGateway::RestApi`
  * `AWS::ApiGateway::Resource`（每条 path 一层）
  * `AWS::ApiGateway::Method`（GET/POST 等）
  * `AWS::ApiGateway::Integration`（连 Lambda）
  * 再加 Deployment/Stage

所以不是“CloudFormation 在 Properties 里写 paths”这么简单；更常见是 **用 Resource/Method 对象堆出来**（非常啰嗦）。

---

### 差异 B：Deployment/Stage（SAM 自动 vs CFN 手动）

你文档说：

> SAM 不需要 deployment 资源；CloudFormation 需要显式定义 deployment 才有 endpoint

✅ 这个说法**非常正确**，而且你这次部署日志已经验证了：

SAM 自动生成了：

* `AWS::ApiGateway::Deployment`
* `AWS::ApiGateway::Stage`

而纯 CloudFormation 里你必须自己写，否则：

* 只创建了 API 和 methods
* 但没有发布（deployment）
* 你就拿不到可用的 invoke URL

---

## 3) 你可以背的“总结一句话”（面试/考试版）

> **SAM 把 API Gateway 的底层资源（Resource/Method/Deployment/Stage/权限）自动生成并管理**；
> 你只要用 `AWS::Serverless::Api` + OpenAPI（或 Function Events）描述意图即可。
> 纯 CloudFormation 必须显式创建 REST API、资源路径、方法、集成、部署与 stage 才能得到 endpoint。

---

## 4) 小提醒：你这套方案里还有一个现实问题

你现在用 OpenAPI 的 `credentials` 让 API Gateway assume `ClabAPIRole` 去 invoke Lambda。
真实项目里更常见的是用 **Lambda::Permission**（让 Lambda 允许 apigateway invoke），权限边界更清晰、也更少“共享角色”的风险。

但 lab 这么写是为了教学统一。

---

如果你想，我可以把“纯 CloudFormation 版本需要写哪些资源”用一个最小骨架列出来（不啰嗦，但让你一眼看懂为什么 CFN 麻烦）。
