下面给你一整套 **👉 可直接复制到 GitHub 的 Markdown 内容**，覆盖你要的 **3 个部分**：
**总架构图 + 高频陷阱题 + 一页速记卡**，完全按 **AWS 考试（SAA/SOA/DVA）取向**设计。

---

# 🌐 Amazon CloudFront 一页速记卡（Exam Ready）

> 目标：**看到“低延迟 / 全球用户 / 静态内容 / CDN”，立刻想到 CloudFront + Edge**

---

## 一、🧠 CloudFront + Edge + Region 总架构图（ASCII）

```text
                    🌍 Global Users
        (Browser / Mobile / Client Worldwide)
                             |
                             v
                   ┌──────────────────┐
                   │  Edge Location   │
                   │ (CloudFront POP) │
                   │  Cache Hit?      │
                   └───────┬──────────┘
                           │ Miss
                           v
        ┌─────────────────────────────────────────┐
        │               AWS Region                │
        │                                         │
        │   ┌───────────────┐    ┌──────────────┐│
        │   │   S3 Origin   │    │  ALB / EC2   ││
        │   │ (Static)      │    │  (Dynamic)   ││
        │   └───────────────┘    └──────────────┘│
        │              ▲                ▲         │
        │              └────── Origin ──┘         │
        └─────────────────────────────────────────┘
                             |
                             v
                   Response cached at Edge
                   (TTL / Cache-Control)
```

📌 **一句话理解**：
👉 **用户 → 最近的 Edge Location →（缓存命中直接返回）→ 否则回源到 Region → 再缓存到 Edge**

---

## 二、CloudFront 核心组件速览

| 组件                | 作用                                 |
| ----------------- | ---------------------------------- |
| **Edge Location** | 靠近用户，缓存内容，降低延迟                     |
| **Origin**        | 内容来源（S3 / ALB / EC2 / API Gateway） |
| **Distribution**  | CloudFront 的配置实体                   |
| **Cache**         | 边缘缓存（TTL 控制）                       |
| **TTL**           | 缓存存活时间                             |
| **Invalidation**  | 强制刷新缓存                             |

---

## 三、🧠「关键词 → 秒选」反射表（核心）

| 题目关键词                            | 秒选                    |
| -------------------------------- | --------------------- |
| **low latency / global users**   | CloudFront            |
| **static content**               | CloudFront + S3       |
| **dynamic content acceleration** | CloudFront + ALB      |
| **CDN**                          | CloudFront            |
| **cache at edge**                | CloudFront            |
| **reduce load on origin**        | CloudFront            |
| **HTTPS globally**               | CloudFront            |
| **DDoS protection at edge**      | CloudFront (+ Shield) |

---

## 四、🧪 CloudFront 高频考试陷阱题（5 道）

---

### ❌ 陷阱题 1

**题目**：

> Improve latency for global users accessing static images.

❌ 错误选项：

* Deploy EC2 in multiple AZs

✅ 正确答案：
👉 **Use CloudFront with S3 origin**

📌 原因：

* AZ 提高可用性，不降低全球延迟

---

### ❌ 陷阱题 2

**题目**：

> CloudFront can only be used with S3.

❌ 错误理解：

* CloudFront = 只能 S3

✅ 正确认知：
👉 **CloudFront 支持 S3、ALB、EC2、API Gateway**

---

### ❌ 陷阱题 3

**题目**：

> Cache dynamic content at Edge Locations.

❌ 错误选项：

* CloudFront cannot serve dynamic content

✅ 正确答案：
👉 **CloudFront can accelerate dynamic content (no caching or short TTL)**

📌 原因：

* CloudFront ≠ 只能静态
* 动态也能加速（但不一定缓存）

---

### ❌ 陷阱题 4

**题目**：

> Immediately update a file served by CloudFront.

❌ 错误选项：

* Wait for TTL expiration only

✅ 正确答案：
👉 **Create a CloudFront invalidation**

---

### ❌ 陷阱题 5

**题目**：

> Protect your application from DDoS attacks globally.

❌ 错误选项：

* Use Security Groups only

✅ 正确答案：
👉 **CloudFront + AWS Shield (at Edge)**

---

## 五、CloudFront 考试“反直觉”总规则（必背）

```text
❗ CloudFront ≠ 只支持 S3
❗ Edge Location ≠ AZ / Region
❗ 低延迟 = Edge
❗ 高可用 = Multi-AZ
❗ 缓存失效 = Invalidation
```

---

## 六、30 秒考前自检清单

* 题目在问 **延迟** 还是 **可用性**？
* 用户是否 **全球分布**？
* 内容是 **静态还是动态**？
* 有没有提 **CDN / cache / edge**？

---

## ✅ 一句话终极总结（考试 / 面试通用）

> Amazon CloudFront 是 AWS 的全球内容分发网络，通过 Edge Locations 缓存和加速内容，将请求就近处理，从而降低延迟、减少源站负载，并提升全球用户体验。

---

### 下一步我可以继续给你（同样 Markdown 可复制）：

* 🔥 **CloudFront + S3 静态网站完整架构速记**
* 🧪 **10 道 CloudFront + S3 / ALB 混合陷阱题**
* 🧠 **CDN / Cache / TTL / Invalidation 决策树**

你直接选一句：
👉 **“要 PDF” / “再来 10 道陷阱题” / “做决策树”**
