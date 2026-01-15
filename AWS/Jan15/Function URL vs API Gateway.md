## 🧪 Function URL vs API Gateway（超高频对比，面试一眼秒）

| 维度                | **Lambda Function URL**                                                                                                     | **API Gateway (HTTP API / REST API)**                                                 |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| 适用场景              | **“一支 Lambda 快速暴露成 HTTPS”**：internal webhook、简单后端、低治理成本                                                                     | **“生产级 API 门面”**：多路由、多后端、鉴权、限流、版本、治理                                                  |
| 路由能力              | 基本就是一个函数入口（函数内自己分 path）                                                                                                     | **原生路由/方法/阶段/版本**（HTTP API 更轻，REST API 更全） ([AWS Documentation][1])                   |
| 安全/鉴权             | `AuthType: NONE` 或 `AWS_IAM`（SigV4）+ 资源策略；**NONE 很危险** ([AWS Documentation][2])                                             | IAM / Cognito / Lambda Authorizer / API Key 等（REST API 功能更全） ([AWS Documentation][1]) |
| WAF / DDoS / 边缘保护 | 不像 API GW 那样“自带一堆门禁”；通常要 **CloudFront** 去做防护层 ([Amazon Web Services, Inc.][3])                                              | API Gateway 常被用作“安全入口”（尤其 REST API 功能更完整） ([AWS Documentation][1])                    |
| CORS / Preflight  | 可配置 CORS，但依然要注意 OPTIONS/headers/credentials（和你前面 CORS 坑一致） ([Amazon Web Services, Inc.][4])                                 | 也可配 CORS；HTTP API/REST API 都是常规做法                                                     |
| 成本/复杂度            | **最省配置**、通常也更省钱（但功能少） ([AWS Bites][5])                                                                                      | 功能多、治理强；HTTP API 通常比 REST API 更便宜 ([Amazon Web Services, Inc.][6])                    |
| 一个细节（2025+变化）     | **从 2025 年 10 月起**：新 Function URL 可能需要 `lambda:InvokeFunctionUrl` **和** `lambda:InvokeFunction` 权限 ([AWS Documentation][2]) | 不适用                                                                                   |

**面试一句话：**

> Function URL 是“最短路径把 Lambda 暴露成 HTTPS”；API Gateway 是“生产级 API 门面（鉴权/限流/治理/多路由）”。 ([AWS Documentation][1])

---

## 🔁 Lambda 返回 **HTML vs JSON**（最稳写法）

### ✅ 返回 JSON（API 常用）

```js
export const handler = async () => {
  return {
    statusCode: 200,
    headers: {
      "Content-Type": "application/json; charset=utf-8",
      // 如需要跨域（浏览器前端调用），再加 CORS 头
      "Access-Control-Allow-Origin": "https://your-frontend.com",
    },
    body: JSON.stringify({ ok: true, message: "hello" }),
  };
};
```

### ✅ 返回 HTML（简单页面/回调落地页）

```js
export const handler = async () => {
  const html = `<!doctype html>
<html><head><meta charset="utf-8"><title>Hello</title></head>
<body><h1>Hello from Lambda</h1></body></html>`;

  return {
    statusCode: 200,
    headers: {
      "Content-Type": "text/html; charset=utf-8",
    },
    body: html,
  };
};
```

> **关键点**：一定要配对 `Content-Type`，否则浏览器/客户端可能误解析。

---

## 🧠 把这一步改写成「生产级安全版本」（给你 3 套常用架构）

你要的“生产级安全”，核心就是：**不要裸奔公网入口**、要有 **鉴权 + 限流/防刷 + 可观测 + 最小权限**。

### 方案 A（最推荐的“轻生产”）：**API Gateway HTTP API + Lambda**

适合：公开 API、前端调用、需要 CORS、需要标准路由/限流（多数情况够用） ([AWS Documentation][1])
生产级加固清单：

* 鉴权：Cognito / IAM / Lambda Authorizer（按业务选）
* 限流：Usage plan（REST API 更强）或上游限流策略
* 日志：Access logs + Lambda logs + tracing
* CORS：统一在 API GW 层配置（避免函数内散落）
* 最小权限 IAM：Lambda role 只给必须的 AWS API

### 方案 B（Function URL 也想“生产化”）：**CloudFront + Function URL (AWS_IAM)**

适合：你想保留 Function URL 的简单，但又要边缘防护/更安全
做法：

* Function URL 用 `AWS_IAM`（拒绝匿名） ([AWS Documentation][2])
* 前面加 CloudFront（抗 DDoS/边缘入口），必要时 Lambda@Edge 做 header/鉴权增强 ([Amazon Web Services, Inc.][3])
* 资源策略只允许特定 principal 调用（别给 `*`）

### 方案 C（企业更常见）：**CloudFront + API Gateway + WAF + Lambda**

适合：强合规、强防护、黑白名单、Bot 防护、地理限制等（WAF 在这层更自然） ([AWS Documentation][1])

---

## ✅ “生产级安全版本”最小落地 Checklist（你直接照抄就行）

1. **入口鉴权**

* 公开给浏览器：优先 API Gateway + Cognito / Authorizer
* internal webhook：Function URL 也至少用 `AWS_IAM`，不要 `NONE` ([Prisma Cloud Documentation][7])

2. **限制攻击面**

* 能私网就私网（VPC + 私有集成/内部入口）
* 需要公网：加 CloudFront（边缘入口） ([Amazon Web Services, Inc.][3])

3. **速率限制 / 防刷**

* API Gateway 层限流/配额（或前置 WAF/CloudFront 规则）
* Lambda 并发控制（Reserved concurrency）防止被打爆

4. **可观测**

* 结构化日志（requestId、latency、status）
* 指标报警（4xx/5xx、p95/p99、throttles）

5. **最小权限**

* Lambda execution role 只给需要的权限
* 如果是 Function URL：注意 invoke 权限变化（2025/10 后更严格） ([AWS Documentation][2])

---

如果你告诉我你这一步现在用的是哪一种（**Function URL** 还是 **API Gateway**，以及是否要给 **React 前端**调用），我可以把它“直接改成可上线”的具体模板：

* **API Gateway 的 CORS + Authorizer + logs**
* 或 **CloudFront + Function URL (AWS_IAM) + 资源策略**（最小权限版）

[1]: https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html?utm_source=chatgpt.com "Choose between REST APIs and HTTP APIs"
[2]: https://docs.aws.amazon.com/lambda/latest/dg/urls-auth.html?utm_source=chatgpt.com "Control access to Lambda function URLs"
[3]: https://aws.amazon.com/blogs/compute/protecting-an-aws-lambda-function-url-with-amazon-cloudfront-and-lambdaedge/?utm_source=chatgpt.com "Protecting an AWS Lambda function URL with Amazon ..."
[4]: https://aws.amazon.com/blogs/aws/announcing-aws-lambda-function-urls-built-in-https-endpoints-for-single-function-microservices/?utm_source=chatgpt.com "Announcing AWS Lambda Function URLs: Built-in HTTPS ..."
[5]: https://awsbites.com/74-function-urls-vs-api-gateway/?utm_source=chatgpt.com "74. Function URLs vs API Gateway"
[6]: https://aws.amazon.com/api-gateway/pricing/?utm_source=chatgpt.com "Amazon API Gateway Pricing"
[7]: https://docs.prismacloud.io/en/enterprise-edition/policy-reference/aws-policies/aws-general-policies/ensure-aws-authtype-for-your-lambda-function-urls-is-defined?utm_source=chatgpt.com "AWS Lambda function URL AuthType set to NONE"
