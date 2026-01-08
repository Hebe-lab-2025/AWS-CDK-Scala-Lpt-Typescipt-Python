好，这一块是 **SAA-C03 最容易“想多就错”的性能题核心区**。
目标只有一个：**看到性能暗号 → 3 秒内在 Cache / DynamoDB / RDS 里秒选**。

---

# 🔥 Part 1：Cache × DynamoDB × RDS【三选一魔鬼题】

> **唯一判断原则**
> 👉 **“慢的是读？写？还是模型不对？”**

---

### 🕳️ Q1（必考）

**关键词**：sub-millisecond latency
✅ **Cache（ElastiCache / DAX）**
❌ DynamoDB
❌ RDS

👉 暗号翻译：**“必须比数据库还快”**

---

### 🕳️ Q2

**关键词**：read-heavy + relational schema
✅ **RDS + Read Replica / Cache**
❌ DynamoDB（模型不匹配）

---

### 🕳️ Q3（反直觉）

**关键词**：read performance issue
❌ 升级 RDS 实例
✅ **Cache in front of DB**

👉 **性能题第一坑：别先升规格**

---

### 🕳️ Q4

**关键词**：massive scale + key-value
✅ **DynamoDB**
❌ RDS（先天不擅长）

---

### 🕳️ Q5

**关键词**：frequently accessed data
✅ **Cache**
❌ DynamoDB（不是“频率”优化）

---

### 🕳️ Q6（高频）

**关键词**：relational + HA
✅ **RDS Multi-AZ**
❌ DynamoDB（不是关系型）

---

### 🕳️ Q7

**关键词**：hot keys / hot partitions
✅ **Cache / DAX**
❌ DynamoDB alone

---

### 🕳️ Q8（反直觉）

**关键词**：performance suddenly drops
✅ **检查 cache miss / eviction**
❌ 扩容 DB

---

### 🕳️ Q9

**关键词**：millions of requests per second
✅ **DynamoDB**
❌ RDS

---

### 🕳️ Q10（总结题）

**关键词**：low latency + cost-effective
✅ **Cache**
❌ 更大的数据库

---

📌 **一句话验收**

> 你如果已经能做到：
> **“性能慢 → 第一反应是 Cache，而不是 DB”**
> 👉 这一章基本送分。

---

# 🧠 Part 2：SAA-C03【性能优化“暗号词典”】【全集版】

> **看到这些词，答案已经写好了**

---

## ⚡ 延迟类

| 暗号              | 秒选          |
| --------------- | ----------- |
| sub-millisecond | Cache / DAX |
| low latency     | Cache       |
| fast read       | Cache       |
| hot data        | Cache       |

---

## 📈 流量类

| 暗号            | 秒选                   |
| ------------- | -------------------- |
| read-heavy    | Read Replica / Cache |
| write-heavy   | Scale DB / DynamoDB  |
| burst traffic | Cache / Queue        |

---

## 🧾 数据模型类

| 暗号          | 秒选       |
| ----------- | -------- |
| relational  | RDS      |
| key-value   | DynamoDB |
| schema-less | DynamoDB |
| joins       | RDS      |

---

## 💰 成本 & 运维

| 暗号             | 秒选                     |
| -------------- | ---------------------- |
| cost-effective | Cache                  |
| least ops      | DynamoDB / Cache       |
| managed        | DynamoDB / ElastiCache |

---

## 🕳️ 反直觉暗号（必记）

| 题干                    | 真正含义                 |
| --------------------- | -------------------- |
| “performance issue”   | **不是扩容 DB**          |
| “scalability problem” | 先想 **NoSQL / Cache** |
| “database overloaded” | **前面挡一层 Cache**      |

---

# 📄 Part 3：Caching Domain【一页 PDF｜考前 5 分钟】

> 下面内容 **= 一页 A4 = 你考前最后该看的东西**

---

## 🧠 三选一决策树（PDF 中央）

```text
性能慢？
 ├─ 读慢？
 │     └─ Cache
 │
 ├─ 写慢？
 │     ├─ Relational → RDS Scale
 │     └─ Key-Value → DynamoDB
 │
 └─ 模型不对？
       └─ DynamoDB（别硬用 RDS）
```

---

## ⚡ Cache 使用场景（必背）

* 热点数据
* 高频读
* 会话 / token
* 排行榜 / 计数器

---

## 🧾 DynamoDB 使用场景

* 超高并发
* Key-value
* Serverless
* 自动扩缩

---

## 🗄️ RDS 使用场景

* 关系型
* 事务
* Join
* 强一致（主库）

---

## 📌 5 行终极口诀（PDF 最后一行）

> “性能慢先加 Cache
> 模型不对换 DynamoDB
> 关系数据用 RDS
> 不先升规格
> 不自己造轮子”

---

# 🎯 自检清单（现在你该做到的）

如果你现在看到题目能 **不犹豫地说出**：

* “sub-ms → Cache”
* “read-heavy → Replica / Cache”
* “key-value + scale → DynamoDB”

👉 **Caching Domain = 稳分区**

---

## 🚀 下一步（最后冲刺建议）

1️⃣ **来一套 30 题 Cache / DB 混合地狱卷**
2️⃣ **只刷性能相关“反直觉错题本”**
3️⃣ **90 分钟全真模考（只剩易错点）**

你直接回 **1 / 2 / 3**，我继续给你拉到满分线。
