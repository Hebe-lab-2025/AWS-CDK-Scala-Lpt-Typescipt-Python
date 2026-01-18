下面这段 Lab 的核心意思（翻译 + 你做完应该掌握什么）我帮你浓缩成一张「学习地图」。

---

## 这节 Lab 在做什么（超短版）

你会用 **API Gateway** 搭一个 **REST API**，不写后端服务器，直接把 API Gateway **集成到 DynamoDB**（Service Integration），实现一套可用的 CRUD 接口，然后部署并调用验证。

---

## 你会做的 4 件事（按实验流程）

1. **建 DynamoDB 表**

   * 设计主键（Partition Key / 可选 Sort Key）
2. **用 Python SDK（boto3）往表里写数据**

   * 先准备几条记录用于测试
3. **建 API Gateway REST API**

   * 配资源路径（Resources）、方法（GET/POST/PUT/DELETE）
4. **把 API Gateway 直接连到 DynamoDB**

   * 配 Integration、Mapping（请求/响应映射）
   * Deploy 后用 curl / Postman 调接口看结果

---

## 你必须搞懂的关键点（考试/面试高频）

### 1) API Gateway 的 “Service Integration”

* **不用 Lambda**，API Gateway 直接调用 AWS 服务（这里是 DynamoDB）
* 好处：更少组件、成本低、延迟低
* 代价：映射配置（Mapping Template）更绕、调试更依赖日志

### 2) “Data Mapping / Mapping Template”

* 你发的是 **HTTP JSON**
* DynamoDB 期望的是 **它自己的 JSON 格式**（AttributeValue：S/N/BOOL/L/M…）
* 所以要做 **请求映射** 和 **响应映射**

### 3) IAM 权限（最容易卡住）

* API Gateway 要有权限调用 DynamoDB
* 关键是：**Integration Role / 执行角色** 是否允许 `dynamodb:PutItem/GetItem/Query/Scan/UpdateItem/DeleteItem`

### 4) REST API 的资源设计（路径 + 方法）

典型设计：

* `POST /items` 创建
* `GET /items/{id}` 查询
* `PUT /items/{id}` 更新
* `DELETE /items/{id}` 删除

---

## 常见坑（你一踩就知道我说的对）

* **主键设计不对**：导致你只能 Scan（慢+贵），不能 Query（快）
* **请求映射写错**：DynamoDB 报 ValidationException
* **响应映射没处理**：返回一堆 DynamoDB AttributeValue，看起来“很丑”
* **没 Deploy / Deploy 到错 Stage**：你改了配置但调用还是旧的
* **权限缺一条 Action**：403/500，CloudWatch log 才看得出原因

---

## 一句话“背诵版”

> This lab builds a REST API in API Gateway and directly integrates it with DynamoDB using service integration and mapping templates, without using Lambda.

---

如果你下一步想更快通关：把你 Lab 里**表的主键设计**（Partition/Sort key）和你要做的 **API 路径**贴出来，我可以直接给你一套「最省事的 CRUD 设计 + 典型 Mapping 思路 + 易错点对照」。


**“monitor the performance” 的翻译与解释：**

### 翻译

* **监控性能**
* **监控系统性能**
* **对性能进行监控**

### 技术语境解释

在 AWS / 系统架构里，**monitor the performance** 通常指：

* 监控 **延迟（Latency）**
* 监控 **吞吐量（Throughput / TPS）**
* 监控 **错误率（4xx / 5xx）**
* 监控 **资源使用情况**（CPU、内存、并发数）

### AWS 场景常用一句

> *We use CloudWatch to monitor the performance of the API.*
> **我们使用 CloudWatch 来监控 API 的性能。**

### 面试一句话版

> **monitor the performance = 用指标和日志持续观察系统是否满足 SLA**


