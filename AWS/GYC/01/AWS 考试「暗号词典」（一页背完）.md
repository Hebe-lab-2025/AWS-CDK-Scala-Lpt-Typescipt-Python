下面这一页就是 **AWS SAA-C03 / 面试「暗号 → 条件反射」全集**。
目标：**看到词 → 不思考 → 直接秒选**。

---

# 一、AWS 考试「暗号词典」（一页背完）

## 🔐 安全 / 身份

| 暗号词                          | 条件反射答案         |
| ---------------------------- | -------------- |
| **least privilege**          | IAM Policy     |
| **temporary credentials**    | IAM Role / STS |
| **no long-term keys on EC2** | IAM Role       |
| **DDoS protection**          | Shield         |
| **SQL injection / XSS**      | WAF            |

---

## 🌐 网络 / VPC

| 暗号词                              | 条件反射答案            |
| -------------------------------- | ----------------- |
| **private subnet outbound only** | NAT Gateway       |
| **cannot access internet**       | Route Table / IGW |
| **public vs private subnet**     | 看 Route Table     |
| **instance-level firewall**      | Security Group    |
| **subnet-level firewall**        | NACL              |
| **high availability**            | Multi-AZ          |

---

## ⚙️ 计算 / 扩展

| 暗号词                       | 条件反射答案       |
| ------------------------- | ------------ |
| **unpredictable traffic** | Auto Scaling |
| **stateless web tier**    | ALB + ASG    |
| **no server management**  | Lambda       |
| **long-running task**     | EC2 / ECS    |
| **burst traffic**         | ASG / Queue  |

---

## 💾 存储

| 暗号词                    | 条件反射答案         |
| ---------------------- | -------------- |
| **object storage**     | S3             |
| **shared filesystem**  | EFS            |
| **boot volume / disk** | EBS            |
| **cheap + durable**    | S3             |
| **cross-region DR**    | S3 Replication |

---

## 🗄️ 数据库 / 缓存

| 暗号词                          | 条件反射答案             |
| ---------------------------- | ------------------ |
| **relational HA**            | RDS Multi-AZ       |
| **read-heavy workload**      | Read Replica       |
| **sub-millisecond latency**  | ElastiCache        |
| **DynamoDB ultra-fast read** | DAX                |
| **CPU 100%**                 | 慢 SQL / 连接数 / 实例太小 |

---

## 🔁 解耦 / 异步

| 暗号词                      | 条件反射答案            |
| ------------------------ | ----------------- |
| **decouple components**  | SQS               |
| **buffer traffic spike** | Queue             |
| **fan-out**              | SNS               |
| **event-driven**         | EventBridge / SNS |

---

# 二、10 道「关键词秒杀题」（不解释，练直觉）

1️⃣ **Unpredictable traffic, high availability**
👉 **ALB + Auto Scaling**

2️⃣ **Private EC2 needs internet, no inbound**
👉 **NAT Gateway**

3️⃣ **Highly durable image storage**
👉 **S3**

4️⃣ **Relational DB with automatic failover**
👉 **RDS Multi-AZ**

5️⃣ **Microsecond latency for DynamoDB reads**
👉 **DAX**

6️⃣ **Decouple web tier from background jobs**
👉 **SQS**

7️⃣ **Shared storage across multiple EC2**
👉 **EFS**

8️⃣ **Block SQL injection attacks**
👉 **WAF**

9️⃣ **Temporary credentials for EC2**
👉 **IAM Role**

🔟 **Reduce DB load, ultra-low latency**
👉 **ElastiCache**

---

# 三、考前速记卡（最后 5 分钟只看这个）

```
Compute:
- Stateless web → ALB + ASG
- No ops → Lambda
- Long jobs → EC2 / ECS

Network:
- Public vs Private → Route Table
- Outbound only → NAT
- SG = Stateful
- NACL = Stateless

Database:
- HA → Multi-AZ
- Scale reads → Read Replica
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

## ❌ 常见错误选项

> “Use an EC2 instance to handle traffic spikes”

### 为什么这是 **考试必错项**

1️⃣ **EC2 本身不会自动扩展**
2️⃣ **需要你自己运维（补丁 / 扩容 / HA）**
3️⃣ **默认是单点（unless ASG）**
4️⃣ **不符合考试关键词：scalable / highly available / minimal ops**

---

## ✅ 考试正确思维链

```
看到：
- unpredictable traffic
- scalable
- highly available
- minimal operational overhead

立刻：
❌ 单台 EC2
✅ 托管 + 自动扩展
→ ALB + ASG（或 Lambda）
```

📌 **考试杀手句（直接背）**

> If scalability and availability are required, EC2 alone is almost never the best answer.

---

# 五、终极心法（3 句话）

1️⃣ **看到 scalable / unpredictable → 自动扩展**
2️⃣ **看到 low latency / offload DB → Cache**
3️⃣ **看到“单台 EC2” → 先默认它是错的**

---

如果你愿意，下一步我可以继续：

* 🔥 拆 **20 道「EC2 误选陷阱题」**
* 🧪 做 **关键词盲选模拟小卷**
* 🧠 练 **暗号 → 架构图 反射训练**
* 🎯 模拟 **真实考试读题 → 秒选全过程**

你选一个，我直接开练。
