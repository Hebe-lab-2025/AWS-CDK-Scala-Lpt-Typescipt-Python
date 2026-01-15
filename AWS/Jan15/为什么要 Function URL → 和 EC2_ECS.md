这一节 **在教你：如何把 Lambda 当成“无服务器网页 / HTTP 接口”来用**。
我按 **发生了什么 → 为什么要 Function URL → 和 EC2/ECS 的对比 → 面试说法** 给你讲清。

---

## 🧠 一句话结论

> **Lambda Function URL = 给 Lambda 一个公网 HTTP 入口，让函数像 Web 服务一样被访问。**

---

## 1️⃣ 你这一节实际做了什么？

### ✅ ① 更新 Lambda 代码

* 改 `index.mjs`
* 返回一个 **HTML / JSON 响应**
* 点 **Deploy**
  👉 等价于：**部署新版本代码**

---

### ✅ ② 创建 Function URL

* 位置：Configuration → Function URL
* Auth type：**NONE**
* AWS 自动生成一个 **HTTPS URL**

👉 这个 URL 就是：

> **触发 Lambda 的入口点（entry point）**

---

### ✅ ③ 浏览器访问 Function URL

```
Browser
  ↓ HTTP GET
Lambda Function
  ↓
HTML / JSON response
```

你已经完成了：

> **“Serverless Web App 的最小闭环”**

---

## 2️⃣ 什么是 Lambda Function URL？

AWS Lambda

**定义**

* AWS 提供的 **内置 HTTP(S) 触发方式**
* 不需要：

  * API Gateway
  * Load Balancer
  * EC2

**特点**

* AWS 托管
* 自动扩缩容
* 按请求计费

---

## 3️⃣ Function URL vs API Gateway（很重要）

| 对比    | Function URL | API Gateway |
| ----- | ------------ | ----------- |
| 配置复杂度 | ⭐ 很简单        | ⭐⭐⭐         |
| 鉴权    | 简单           | 非常强         |
| 路由    | 单一           | 多路由         |
| 适合    | Demo / Lab   | 生产系统        |

👉 **你这个 Lab 用 Function URL 是教学最优解**

---

## 4️⃣ event / response 在干嘛？

### event 是什么？

* HTTP 请求信息

  * method
  * headers
  * query params
  * body

### response 是什么？

```json
{
  "statusCode": 200,
  "body": "Hello World"
}
```

👉 Lambda = **请求 → 函数 → 返回**

---

## 5️⃣ 和你刚做的 EC2 对比（核心理解点）

| 维度    | EC2       | Lambda       |
| ----- | --------- | ------------ |
| 登录服务器 | 需要        | 不需要          |
| 管 OS  | 需要        | 不需要          |
| 部署方式  | SSH / npm | Deploy       |
| 访问方式  | IP:Port   | Function URL |
| 扩容    | 手动        | 自动           |

💡 这就是 AWS 想让你感受到的：

> **“从服务器 → Serverless 的抽象跃迁”**

---

## 6️⃣ 架构图（直观理解）

![Image](https://miro.medium.com/1%2AmOkTJ-KPpsd16h04KQjFYA.png)

![Image](https://miro.medium.com/1%2AXSiiYv0oG8tuVwvtWpS7iA.png)

```
User Browser
     ↓
Function URL (HTTPS)
     ↓
Lambda Function
     ↓
Response
```

---

## 7️⃣ 面试级一句话总结（直接背）

**中文**

> 我通过 Lambda Function URL 为 Lambda 提供了一个公网 HTTP 入口，实现了一个无需服务器的 Web 应用。

**英文**

> I used a Lambda Function URL to expose my Lambda function as an HTTP endpoint and deploy a simple serverless web application.

---

## 8️⃣ AWS 想通过这一节让你“悟到”的点

* 你不需要服务器也能跑 Web
* 你不需要端口 / Security Group
* 可用性、扩容 AWS 全包

👉 **这一步是在为你后面理解：API Gateway / Serverless 架构打基础**

---

如果你愿意，我可以下一步帮你：

* 🧠 用一句话区分 **Lambda Function URL vs API Gateway**
* 🔄 把这个 Lambda 接到 **前端 / ECS / EC2**
* 🧪 出 5 道 **Serverless 面试追问题**

你想 **继续 Lab** 还是 **开始整理面试答案**？
