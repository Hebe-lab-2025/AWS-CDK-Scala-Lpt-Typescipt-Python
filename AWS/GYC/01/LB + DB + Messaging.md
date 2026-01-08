好，这一轮我**完全按真考节奏**来：
**30 秒反射 × 高密度暗号 × 专治过度设计**。
你只需要做一件事：**看到关键词 → 条件反射选服务**。

---

# 🔥 Part 1：30 秒 × 20 轮【LB + DB + Messaging】混合直觉训练

> **规则**
>
> * 每题 ≤ 2 行
> * 不允许“自己设计系统”
> * 只选 **AWS 托管最小解**

---

### 🔁 Round 1

**关键词**：HTTP + path-based routing + EC2
✅ **ALB**
❌ NLB / API Gateway

---

### 🔁 Round 2

**关键词**：global users + static images + low latency
✅ **CloudFront**
❌ ALB alone

---

### 🔁 Round 3

**关键词**：bursty traffic + backend overload
✅ **SQS buffer**
❌ 直接 ASG 扩容

---

### 🔁 Round 4（反直觉）

**关键词**：sub-millisecond latency
✅ **ElastiCache / DAX**
❌ RDS Read Replica

---

### 🔁 Round 5

**关键词**：write-heavy + relational + HA
✅ **RDS Multi-AZ**
❌ Read Replica（不管写）

---

### 🔁 Round 6

**关键词**：read-heavy + scaling
✅ **Read Replica / Cache**
❌ 提升主库规格

---

### 🔁 Round 7

**关键词**：fan-out + multiple consumers
✅ **SNS → SQS**
❌ 一个 SQS 给所有人

---

### 🔁 Round 8

**关键词**：strict order + exactly once
✅ **SQS FIFO**
❌ Standard（再便宜也错）

---

### 🔁 Round 9（反直觉）

**关键词**：Lambda 执行多次
✅ **at-least-once 语义（幂等性问题）**
❌ AWS bug

---

### 🔁 Round 10

**关键词**：EC2 scale + traffic distribution
✅ **ALB + ASG**
❌ Route 53 alone

---

### 🔁 Round 11

**关键词**：database connection timeout
✅ **先查 VPC / SG / NACL**
❌ 直接升 DB 实例

---

### 🔁 Round 12

**关键词**：cost-effective + high throughput messaging
✅ **SQS Standard**
❌ FIFO（没说顺序）

---

### 🔁 Round 13

**关键词**：decouple microservices
✅ **SQS / SNS**
❌ 同步 REST

---

### 🔁 Round 14（反直觉）

**关键词**：auto scaling slow
✅ **Warm Pool / Predictive Scaling**
❌ 更大的实例

---

### 🔁 Round 15

**关键词**：API + JWT + throttling
✅ **API Gateway**
❌ ALB

---

### 🔁 Round 16

**关键词**：WebSocket
✅ **ALB / API Gateway**
❌ CloudFront

---

### 🔁 Round 17

**关键词**：database failover
✅ **Multi-AZ automatic failover**
❌ Read Replica promotion（慢）

---

### 🔁 Round 18

**关键词**：retry + poison message
✅ **DLQ**
❌ 无限重试

---

### 🔁 Round 19（反直觉）

**关键词**：session lost after scaling
✅ **Stateless + external store**
❌ Sticky Session（临时方案）

---

### 🔁 Round 20（收官题）

**关键词**：reliable + scalable + minimal ops
✅ **ALB + ASG + RDS Multi-AZ + SQS**
❌ EC2 + cron + scripts

---

📌 **验收标准**

> 20 题里 ≥ 16 题不用犹豫 → 这一块就是送分

---

# 🕳️ Part 2：全套【反直觉挖坑题】（专治过度设计）

### 🕳️ 坑 1

**题干**：高并发 → 扩容 DB
✅ 正解：**缓存 / 读副本**
❌ 错因：一上来就升规格

---

### 🕳️ 坑 2

**题干**：消息重复
✅ 正解：**幂等性 + FIFO（如需要）**
❌ 错因：怪 AWS

---

### 🕳️ 坑 3

**题干**：延迟高
✅ 正解：**CloudFront / Cache**
❌ 错因：换实例类型

---

### 🕳️ 坑 4

**题干**：系统不稳定
✅ 正解：**Queue 解耦**
❌ 错因：同步调用链

---

### 🕳️ 坑 5（必考）

**题干**：most scalable, least operational overhead
✅ 正解：**托管服务**
❌ 错因：自建系统

---

📌 **考试真相一句话**

> **AWS 考试不是考你会不会设计系统，而是会不会“不设计系统”**

---

# 📄 Part 3：SAA-C03【一页决策树】考试版（可直接背）

```text
入口？
 ├─ Web / EC2 ──> ALB
 ├─ Global / Static ──> CloudFront
 ├─ API / JWT ──> API Gateway

流量不稳？
 ├─ 请求 ──> ASG
 └─ 任务 ──> SQS → Lambda

数据库慢？
 ├─ 写 ──> Multi-AZ
 └─ 读 ──> Read Replica / Cache

消息？
 ├─ 广播 ──> SNS
 ├─ 缓冲 ──> SQS
 ├─ 顺序 ──> FIFO
 └─ 失败 ──> DLQ
```

### 📌 5 行必背口诀（临考）

> “入口先选 ALB
> 全球加 CloudFront
> 流量先排队
> DB 先分读写
> 顺序才 FIFO”

---

## 🚀 下一步（你现在最适合的）

1️⃣ **来一整套 65 题“全是坑”的终极模拟卷**
2️⃣ **只做错题：LB + DB + Messaging 错因本**
3️⃣ **90 分钟真考模式模拟**

你直接选 **1 / 2 / 3**，我立刻继续。
