太好了，这一轮我们**专打 Cache / DB 的“反直觉地狱区”**，目标只有一个：

> **看到性能暗号 → 不升级 DB → 立刻想到 Cache / 架构调整**

下面我按你 3 个点 **直接交付成品**。

---

# 1️⃣ 30 题 Cache / DB 混合「地狱卷」（只考易错点）

> **规则**
>
> * 全是 **Aurora / RDS / DynamoDB / ElastiCache / DAX** 混合
> * 题干短、陷阱狠
> * 每题都在考：**你会不会下意识“升级数据库”**

---

## 🔥 Q1–Q30（精选地狱题）

### Q1

应用读多写少，RDS CPU 正常但延迟高。
A. 增大 RDS 实例
B. RDS Multi-AZ
C. Read Replica
D. ElastiCache Redis
✅ **D**

---

### Q2

需要 **亚毫秒读取**，数据在 DynamoDB。
A. 增大 RCU
B. DynamoDB Streams
C. DAX
D. ElastiCache
✅ **C**

---

### Q3

数据库偶尔 failover，但性能没提升。
A. Read Replica
B. Multi-AZ
C. ElastiCache
D. Aurora
✅ **B**
👉 **反直觉点**：Multi-AZ ≠ 性能

---

### Q4

突发流量导致 DB 连接数爆炸。
A. 增大 max_connections
B. ASG 扩容
C. ElastiCache
D. Read Replica
✅ **C**

---

### Q5

大量相同查询重复命中数据库。
A. Read Replica
B. Multi-AZ
C. Cache
D. Sharding
✅ **C**

---

### Q6

写压力大，读压力一般。
A. Read Replica
B. Cache
C. 增大实例
D. Aurora
✅ **C**

---

### Q7

需要强一致读。
A. Read Replica
B. Cache
C. 主库直读
D. DAX
✅ **C**

---

### Q8

读多、可接受轻微过期数据。
A. 主库
B. Multi-AZ
C. Cache
D. 同步复制
✅ **C**

---

### Q9

DynamoDB 热 key 导致 throttling。
A. 增大 RCU
B. DAX
C. 分区键重设计
D. Global Table
✅ **C**

---

### Q10

数据库慢，但 CloudWatch 显示 I/O 很低。
A. 存储不够
B. 网络问题
C. 查询没走索引
D. Cache
✅ **C**

---

### Q11

大量只读 API，请求高度重复。
A. Read Replica
B. Cache
C. Multi-AZ
D. Aurora Global
✅ **B**

---

### Q12

Aurora 性能瓶颈，读写都重。
A. Read Replica
B. Cache
C. Aurora Auto Scaling
D. 拆服务
✅ **C**

---

### Q13

需要跨 Region 低延迟读。
A. Multi-AZ
B. Cache
C. Cross-Region Read Replica
D. Global Accelerator
✅ **C**

---

### Q14

数据库写入慢，读很快。
A. Cache
B. Read Replica
C. 增大实例
D. DAX
✅ **C**

---

### Q15

需要缓存 Session，避免 sticky session。
A. 本地内存
B. RDS
C. ElastiCache
D. DynamoDB Streams
✅ **C**

---

### Q16

DynamoDB 需要 **microsecond-level** 访问。
A. 增大 RCU
B. Cache
C. DAX
D. GSI
✅ **C**

---

### Q17

Cache 命中率很低。
A. 增大 Cache
B. 调整 TTL / key 设计
C. 上 Read Replica
D. Multi-AZ
✅ **B**

---

### Q18

需要确保数据永不丢失。
A. Cache
B. Read Replica
C. Multi-AZ
D. DAX
✅ **C**

---

### Q19

查询复杂、Join 多。
A. DynamoDB
B. Cache
C. RDS / Aurora
D. DAX
✅ **C**

---

### Q20

热点数据访问极高。
A. 主库
B. Read Replica
C. Cache
D. Multi-AZ
✅ **C**

---

### Q21

写入频繁但不要求实时读。
A. Cache
B. 异步写 + Queue
C. Read Replica
D. DAX
✅ **B**

---

### Q22

读压力大，但必须最新数据。
A. Cache
B. Read Replica
C. 主库
D. Multi-AZ
✅ **C**

---

### Q23

需要防止缓存击穿。
A. 提高 TTL
B. Cache aside + lock
C. Multi-AZ
D. Read Replica
✅ **B**

---

### Q24

Cache 挂了怎么办？
A. 服务不可用
B. 自动切回 DB
C. 手动重启
D. Multi-AZ
✅ **B**

---

### Q25

缓存数据频繁变动。
A. Cache
B. Read Replica
C. 不适合 Cache
D. DAX
✅ **C**

---

### Q26

数据库 CPU 高但 QPS 不高。
A. Cache
B. SQL 优化
C. Read Replica
D. 增大实例
✅ **B**

---

### Q27

大量小对象读取。
A. RDS
B. Cache
C. EBS
D. Glacier
✅ **B**

---

### Q28

性能问题只在读路径。
A. Cache
B. Multi-AZ
C. 增大实例
D. Sharding
✅ **A**

---

### Q29

希望减轻 DB 压力、而非提高 DB 性能。
A. Cache
B. Read Replica
C. Multi-AZ
D. Aurora
✅ **A**

---

### Q30

看到题干写：**sub-millisecond latency**
👉 **直接选 Cache / DAX**

---

# 2️⃣ 性能相关「反直觉错题本」（只记这 12 条）

> **这是 Cache / DB 最容易丢分的地方**

### ❌ 误区 1

**Multi-AZ = 性能提升**
✅ 真相：只为 HA

---

### ❌ 误区 2

**Read Replica = 写性能提升**
✅ 真相：只分担读

---

### ❌ 误区 3

**DB 慢 = 升级实例**
✅ 真相：先 Cache / SQL 优化

---

### ❌ 误区 4

**DAX = 所有 DynamoDB 场景**
✅ 真相：只适合读多、最终一致

---

### ❌ 误区 5

**Cache = 永远安全**
✅ 真相：必须有 DB fallback

---

### ❌ 误区 6

**Cache 命中率低 = Cache 太小**
✅ 真相：多半是 key / TTL 设计问题

---

### ❌ 误区 7

**强一致 + 高性能 = Cache**
✅ 真相：冲突，只能主库

---

### ❌ 误区 8

**Read Replica 越多越好**
✅ 真相：写仍是瓶颈

---

### ❌ 误区 9

**Aurora 一定比 RDS 快**
✅ 真相：看 workload

---

### ❌ 误区 10

**Cache 数据必须完全准确**
✅ 真相：能容忍过期才用 Cache

---

### ❌ 误区 11

**DynamoDB 热点靠加 RCU**
✅ 真相：先重设计 partition key

---

### ❌ 误区 12

**Cache 掉了 = 系统挂**
✅ 真相：Cache 是加速层，不是数据源

---

# 3️⃣ 90 分钟「只剩易错点」全真模考方案

## ⏱️ 时间分配（90 分钟）

* **0–10 分钟**
  快速扫题，标记：
  👉 Cache / Replica / DAX 犹豫题

* **10–65 分钟**
  第一遍全做（**1 分钟 / 题**）

* **65–80 分钟**
  只回看这 3 类题：

  * Multi-AZ vs Read Replica
  * Cache vs DB
  * DynamoDB vs DAX

* **80–90 分钟**
  用下面的 **错因模板** 强行定型

---

## 🧠 错因模板（Cache / DB 专用）

```text
Q#：
性能暗号：
我为什么第一反应想升 DB：
真正该做的是：
一句话秒杀规则：
```

### 示例

```text
Q12
暗号：read-heavy + latency
错误直觉：升 DB
正确：Cache
规则：读慢先 Cache，不先升库
```

---

## 🎯 如果你愿意，我可以下一步直接做 3 件事之一：

1️⃣ 把这 30 题做成 **Anki 地狱闪卡（只看暗号）**
2️⃣ 出一套 **Cache / DB 专项 65 题终极卷**
3️⃣ 给你一张 **「性能优化决策树一页图」**（考前 3 分钟）

你现在已经到了 **“知道自己为什么会错”** 的阶段，
这是**最后 5–10 分提分区**。
