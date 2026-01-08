下面给你一套 **「AWS 考试暗号 → 秒选答案」终极训练包**，目标只有一个：**读到关键词，答案自动跳出来**。

---

# 一、AWS 考试【暗号词典】📕（看到就该条件反射）

## 🔐 安全 / 访问

| 暗号词                       | 秒选含义                   |
| ------------------------- | ---------------------- |
| **least privilege**       | IAM Policy / Role      |
| **temporary credentials** | IAM Role / STS         |
| **public access blocked** | S3 Block Public Access |
| **DDoS protection**       | Shield                 |
| **WAF rules**             | Web 攻击（SQLi/XSS）       |

---

## 🌐 网络 / VPC

| 暗号词                              | 秒选含义              |
| -------------------------------- | ----------------- |
| **private subnet outbound only** | NAT Gateway       |
| **cannot access Internet**       | Route Table / IGW |
| **subnet-level firewall**        | NACL              |
| **instance-level firewall**      | Security Group    |
| **multi-AZ high availability**   | 至少 2 个 AZ         |

---

## ⚙️ 计算 / 扩展

| 暗号词                       | 秒选含义                 |
| ------------------------- | -------------------- |
| **unpredictable traffic** | Auto Scaling         |
| **stateless web tier**    | ALB + ASG            |
| **serverless, no ops**    | Lambda               |
| **long-running process**  | EC2 / ECS（不是 Lambda） |
| **milliseconds latency**  | In-memory cache      |

---

## 💾 存储 / 数据

| 暗号词                              | 秒选含义        |
| -------------------------------- | ----------- |
| **shared file system**           | EFS         |
| **object storage**               | S3          |
| **block storage**                | EBS         |
| **frequent access, low latency** | ElastiCache |
| **NoSQL key-value**              | DynamoDB    |

---

## 🗄️ 数据库

| 暗号词                              | 秒选含义               |
| -------------------------------- | ------------------ |
| **HA relational DB**             | RDS Multi-AZ       |
| **read-heavy workload**          | Read Replica       |
| **microsecond latency DynamoDB** | DAX                |
| **CPU 100%**                     | 慢 SQL / 连接数 / 实例太小 |
| **connection timeout**           | 网络（SG/NACL）        |

---

## 🔁 解耦 / 异步

| 暗号词                      | 秒选含义              |
| ------------------------ | ----------------- |
| **decouple components**  | SQS               |
| **fan-out**              | SNS               |
| **event-driven**         | SNS / EventBridge |
| **buffer traffic spike** | Queue             |

---

# 二、10 道【关键词秒杀题】🧠（训练直觉）

### Q1

**Unpredictable traffic spikes, minimal ops overhead**
✅ **ALB + Auto Scaling**

---

### Q2

**Private EC2 needs Internet access, no inbound allowed**
✅ **NAT Gateway**

---

### Q3

**Store images, highly durable, cheap**
✅ **S3**

---

### Q4

**Relational DB, automatic failover required**
✅ **RDS Multi-AZ**

---

### Q5

**Microsecond latency for DynamoDB reads**
✅ **DAX**

---

### Q6

**Decouple web tier from background processing**
✅ **SQS**

---

### Q7

**Shared filesystem across EC2 instances**
✅ **EFS**

---

### Q8

**Block SQL injection attacks**
✅ **WAF**

---

### Q9

**Temporary AWS credentials for EC2**
✅ **IAM Role**

---

### Q10

**Sub-millisecond read latency, DB offload**
✅ **ElastiCache**

---

# 三、考前【一页速记卡】🧾（最后 10 分钟只看这个）

```
Compute:
- Stateless web → ALB + ASG
- Serverless → Lambda
- Long tasks → EC2

Network:
- Public vs Private → Route Table
- Outbound only → NAT
- SG = Stateful / NACL = Stateless

Database:
- HA → Multi-AZ
- Scale read → Read Replica
- Cache → ElastiCache
- DynamoDB accel → DAX

Storage:
- Object → S3
- Block → EBS
- Shared → EFS

Decouple:
- Queue → SQS
- Fan-out → SNS
```

---

# 四、真实错题拆解：**为什么 EC2 是错的？** ❌

### ❌ 常见错误选项：

> **“Use an EC2 instance to handle traffic spikes”**

### 为什么错？

1. **EC2 本身不会自动扩展**
2. **需要手动运维（打补丁、监控、扩容）**
3. **单点风险（unless ASG）**
4. **考试默认追求：高可用 + 自动化**

### ✅ 正确思路应该是：

```
流量不可预测
→ 自动扩展
→ 托管服务
→ ALB + ASG（或 Lambda）
```

📌 **考试杀手句**

> If the question mentions scalability, availability, or minimal operational overhead, EC2 alone is almost never the best answer.

---

# 五、终极考试心法（背这 3 句）

1️⃣ **看到“unpredictable / scalable” → 托管 + 自动化**
2️⃣ **看到“low latency / offload DB” → Cache**
3️⃣ **看到“EC2 单机” → 先怀疑它是错的**

---

如果你愿意，下一步我可以继续帮你：

* 🔥 拆 **20 道“EC2 误选陷阱题”**
* 🧪 出一套 **SAA-C03 关键词盲选小卷**
* 🧠 做 **暗号 → 架构图** 的反射训练
* 🎯 模拟 **真实考试读题 → 秒选全过程**

你想继续 **哪一个？**
