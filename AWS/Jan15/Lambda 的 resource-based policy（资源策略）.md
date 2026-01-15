```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "StatementId": "FunctionURLAllowPublicAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "lambda:InvokeFunctionUrl",
      "Resource": "arn:aws:lambda:us-east-1:214228787083:function:computelab-lambda",
      "Condition": {
        "StringEquals": {
          "lambda:FunctionUrlAuthType": "NONE"
        }
      }
    },
    {
      "StatementId": "FunctionURLAllowInvokeAction",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "lambda:InvokeFunction",
      "Resource": "arn:aws:lambda:us-east-1:214228787083:function:computelab-lambda",
      "Condition": {
        "Bool": {
          "lambda:InvokedViaFunctionUrl": "true"
        }
      }
    }
  ]
}
```
下面这段是 **Lambda 的 resource-based policy（资源策略）**，专门给 **Lambda Function URL** 用的。它的效果一句话：

> **把你的 Lambda Function URL 设成“公网可访问”（AuthType = NONE），任何人都能通过 URL 调用你的 Lambda。**

---

## 1) 这份策略在干嘛（整体解读）

* `Principal: "*"`：**所有人**（公网）
* `Effect: "Allow"`：允许
* `Resource: arn:...:function:computelab-lambda`：只作用在这一个 Lambda 上
* 关键点：它不是 IAM User/Role policy，而是 **挂在 Lambda 资源上的 policy**（谁能调用“这个函数”）

---

## 2) Statement #1：允许调用 **Function URL**（最关键）

```json
{
  "StatementId": "FunctionURLAllowPublicAccess",
  "Effect": "Allow",
  "Principal": "*",
  "Action": "lambda:InvokeFunctionUrl",
  "Condition": {
    "StringEquals": {
      "lambda:FunctionUrlAuthType": "NONE"
    }
  }
}
```

### 含义（直白版）

* 允许任何人执行：`lambda:InvokeFunctionUrl`
* 但前提是：这个 Function URL 的认证类型必须是 **NONE**

  * 也就是：**不需要 AWS IAM 签名、不需要 token**

✅ 所以这条 statement = **公开 URL 访问开关**。

---

## 3) Statement #2：允许 “通过 Function URL 间接触发的 InvokeFunction”

```json
{
  "StatementId": "FunctionURLAllowInvokeAction",
  "Effect": "Allow",
  "Principal": "*",
  "Action": "lambda:InvokeFunction",
  "Condition": {
    "Bool": {
      "lambda:InvokedViaFunctionUrl": "true"
    }
  }
}
```

### 含义

* 允许任何人执行：`lambda:InvokeFunction`
* 但只在一个条件下：**这次调用必须是“通过 Function URL”触发的**

✅ 这条相当于补全权限链：**从 URL 入口进来的调用，最终能落到函数执行上**。

---

## 4) 安全角度：这份 policy 的风险点（你要能说出来）

### ✅ 风险

* 这是**公网开放**：任何人都能打你的 URL
* 可能导致：

  * 被刷请求（DDoS/滥用）
  * 成本飙升（Lambda 调用计费）
  * 业务被探测/攻击（如果函数里处理输入不严谨）

### ✅ 最常见的更安全做法

* 把 `AuthType` 改成 **AWS_IAM**（需要 SigV4 签名）
* 或者放在 **API Gateway** 前面做：

  * Authorizer / JWT / WAF / rate limit
* 或者至少在代码里加：简单 token 校验 + 限流策略

---

## 5) 面试可背一句话

> *“This is a Lambda resource-based policy for Function URL. With AuthType NONE and Principal '*', it allows public invocation via the Function URL, and only permits InvokeFunction when the invocation is coming through that URL.”*

---

如果你把你控制台里 **Function URL 的设置页截图**（尤其是 AuthType / CORS），我可以进一步帮你确认：

* 你现在到底是 **开放公网**还是**需要 IAM**
* CORS 是否会导致前端 403/blocked by CORS 的坑
* 如何改成“可访问但不裸奔”的安全版本
