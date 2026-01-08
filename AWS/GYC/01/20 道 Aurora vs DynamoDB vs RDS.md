好，这一轮是 **SAA-C03 数据库域的“终极杀手锏”**。
目标：**看到题干 3 秒内，在 Aurora / DynamoDB / RDS 里秒选，不犹豫、不想多。**

---

# 🔥 Part 1：20 道 **Aurora vs DynamoDB vs RDS** 三选一魔鬼题

> **唯一心法**
> 👉 先判断 **数据模型**，再看 **并发 & 延迟**，最后看 **HA / 成本**

---

### 🕳️ Q1（必考）

**关键词**：sub-millisecond latency
✅ **DynamoDB + DAX / Cache**
❌ Aurora / RDS

---

### 🕳️ Q2

**关键词**：relational + MySQL compatible
✅ **RDS / Aurora**
❌ DynamoDB

---

### 🕳️ Q3（反直觉）

**关键词**：high availability + MySQL
✅ **Aurora**
❌ RDS Multi-AZ（能，但不最优）

---

### 🕳️ Q4

**关键词**：millions of requests per second
✅ **DynamoDB**
❌ Aurora / RDS

---

### 🕳️ Q5

**关键词**：existing MySQL app + minimal changes
✅ **RDS**
❌ Aurora（要迁移）

---

### 🕳️ Q6（高频）

**关键词**：read-heavy + relational
✅ **Aurora Read Replicas / Cache**
❌ DynamoDB（模型不对）

---

### 🕳️ Q7

**关键词**：serverless + auto scale DB
✅ **DynamoDB**
❌ RDS

---

### 🕳️ Q8（反直觉）

**关键词**：performance bottleneck
✅ **Cache in front of DB**
❌ 换 Aurora（想多了）

---

### 🕳️ Q9

**关键词**：strong consistency + transactions
✅ **RDS / Aurora**
❌ DynamoDB（默认最终一致）

---

### 🕳️ Q10

**关键词**：key-value + unpredictable traffic
✅ **DynamoDB**
❌ RDS

---

### 🕳️ Q11

**关键词**：global read scalability
✅ **Aurora Global Database**
❌ RDS Read Replica（慢）

---

### 🕳️ Q12

**关键词**：OLTP + complex joins
✅ **Aurora**
❌ DynamoDB

---

### 🕳️ Q13（反直觉）

**关键词**：database overloaded
✅ **加 Cache / 分读写**
❌ 直接升 DB 实例

---

### 🕳️ Q14

**关键词**：cost-sensitive + low ops
✅ **DynamoDB**
❌ Aurora（贵）

---

### 🕳️ Q15

**关键词**：failover speed critical
✅ **Aurora（秒级）**
❌ RDS Multi-AZ（分钟级）

---

### 🕳️ Q16

**关键词**：schema-less data
✅ **DynamoDB**
❌ RDS

---

### 🕳️ Q17

**关键词**：existing PostgreSQL app
✅ **RDS PostgreSQL**
❌ DynamoDB

---

### 🕳️ Q18

**关键词**：write-heavy relational workload
✅ **Aurora**
❌ RDS（扩展差）

---

### 🕳️ Q19（反直觉）

**关键词**：low latency requirement
✅ **Cache / DAX**
❌ Aurora（再快也是 DB）

---

### 🕳️ Q20（终极总结）

**关键词**：scalable + reliable + minimal ops
✅ **DynamoDB / Aurora（按模型）**
❌ 自建方案

---

📌 **验收一句话**

> 如果你现在能 **先排除 1 个，再在剩下 2 个里秒选**，这 20 题就是送分。

---

# 🧠 Part 2：SAA-C03【数据库“暗号词典”】【全集】

## 🧾 数据模型暗号（第一步）

| 暗号                      | 秒选           |
| ----------------------- | ------------ |
| relational / joins      | RDS / Aurora |
| key-value               | DynamoDB     |
| schema-less             | DynamoDB     |
| existing MySQL          | RDS          |
| cloud-native relational | Aurora       |

---

## ⚡ 性能 & 并发暗号（第二步）

| 暗号                     | 秒选                   |
| ---------------------- | -------------------- |
| sub-ms latency         | Cache / DAX          |
| millions rps           | DynamoDB             |
| read-heavy             | Read Replica / Cache |
| write-heavy relational | Aurora               |

---

## 🛡️ 高可用 & 灾备暗号（第三步）

| 暗号               | 秒选               |
| ---------------- | ---------------- |
| fastest failover | Aurora           |
| global reads     | Aurora Global DB |
| HA (basic)       | RDS Multi-AZ     |

---

## 💰 成本 & 运维暗号

| 暗号                 | 秒选             |
| ------------------ | -------------- |
| least ops          | DynamoDB       |
| cost-sensitive     | DynamoDB / RDS |
| managed relational | Aurora         |

---

## 🕳️ 反直觉必背

| 题干                | 真正含义         |
| ----------------- | ------------ |
| performance issue | 先想 Cache     |
| scalability issue | 先想 DynamoDB  |
| HA issue          | Aurora > RDS |

---

# 📄 Part 3：**数据库章节 PDF（10 页，专刷）—内容结构**

> 下面就是 **10 页 PDF 的完整骨架**（你可直接导出）

---

### 📘 Page 1｜考试心法

* 数据库题 ≠ SQL 题
* 先模型，后性能，最后 HA

---

### 📘 Page 2｜三选一总览表

* Aurora vs DynamoDB vs RDS 对比

---

### 📘 Page 3｜RDS 专页

* 适用场景
* Multi-AZ vs Read Replica
* 常见坑

---

### 📘 Page 4｜Aurora 专页

* Storage 架构
* Failover 优势
* 什么时候“必须选 Aurora”

---

### 📘 Page 5｜DynamoDB 专页

* Key-value
* Auto scale
* On-demand vs Provisioned（只点名，不深挖）

---

### 📘 Page 6｜性能优化页

* Cache / DAX
* 读写分离
* Hot key 处理

---

### 📘 Page 7｜高并发架构页

* DB + Cache + Queue 组合

---

### 📘 Page 8｜反直觉错题 Top 10

* 为什么你会选错

---

### 📘 Page 9｜3 秒决策表

* 看到关键词直接选

---

### 📘 Page 10｜考前 1 分钟口诀

> “模型不对别硬上
> 性能慢先加 Cache
> 并发极高用 DynamoDB
> 关系复杂选 Aurora
> 不先升规格”

---

# 🧪 Part 4：数据库 × 缓存 × 高并发【组合题】

### 🧪 组合题 1

**关键词**：read-heavy + relational + burst traffic
✅ **Aurora + ElastiCache**

---

### 🧪 组合题 2

**关键词**：event-driven + massive scale
✅ **DynamoDB + SQS + Lambda**

---

### 🧪 组合题 3（反直觉）

**关键词**：DB overloaded
✅ **Cache + Queue 解耦**
❌ 换数据库

---

### 🧪 组合题 4

**关键词**：global users + low latency
✅ **CloudFront + Cache + Aurora**

---

## 🎯 你现在该达到的状态

如果你看到数据库题，**脑子里自动弹出这三步**：
1️⃣ 模型对不对
2️⃣ 读多还是写多
3️⃣ 要不要 Cache

👉 **数据库域 = 稳分区**

---

## 🚀 最后冲刺可继续

1️⃣ **65 题全域真考模拟（数据库权重拉满）**
2️⃣ **只刷数据库 & 性能错题本**
3️⃣ **3 秒口述反射训练（我出题你秒答）**

直接回 **1 / 2 / 3**，我继续陪你冲到线。
