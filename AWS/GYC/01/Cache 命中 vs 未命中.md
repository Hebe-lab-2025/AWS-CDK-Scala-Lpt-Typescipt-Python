下面这套是 **SAA-C03 + 面试** 都通吃的 **缓存核心模块**，我按你给的 4 点 **一条主线讲透**。

---

## 一、Cache 命中 vs 未命中（时序图 · 必考）

### ✅ Cache Hit（命中）

```
Client
  |
  | ① request
  v
App
  |
  | ② GET key
  v
Cache (Redis)
  |
  | ③ value found
  v
App
  |
  | ④ response
  v
Client
```

📌 关键词

* **Low latency**
* **No database call**
* **Sub-millisecond**

---

### ❌ Cache Miss（未命中）

```
Client
  |
  | ① request
  v
App
  |
  | ② GET key
  v
Cache (miss)
  |
  | ③ fallback
  v
Database
  |
  | ④ data
  v
App
  |
  | ⑤ SET cache
  v
Cache
  |
  | ⑥ response
  v
Client
```

📌 关键词

* **Cache miss penalty**
* **Read from DB**
* **Populate cache**

---

## 二、Cache Aside vs Read-Through（核心设计题）

### 1️⃣ Cache Aside（最常考 / 最常用）

```
App → Cache (miss)
App → DB
App → Cache (write)
App → Client
```

**特点**

* 应用自己控制缓存
* 读 / 写逻辑都在 App
* **最灵活，最常见**

**优点**

* 控制力强
* 容易调试
* AWS 官方推荐

**缺点**

* 代码复杂
* 容易忘记更新缓存

📌 **一句考试答案**

> Cache Aside requires the application to manage cache reads and writes.

---

### 2️⃣ Read-Through（抽象化缓存）

```
App → Cache
Cache → DB (miss)
Cache → App
```

**特点**

* App 只读 Cache
* Cache 自动从 DB 加载
* 常见于 **DAX**

**优点**

* App 代码更简单
* 自动填充缓存

**缺点**

* 灵活性低
* 强绑定底层存储

📌 **一句考试答案**

> Read-Through cache automatically loads data from the database on cache miss.

---

### 🧠 对比速记表

| 维度    | Cache Aside | Read-Through |
| ----- | ----------- | ------------ |
| 谁管缓存  | App         | Cache        |
| 灵活性   | ⭐⭐⭐⭐⭐       | ⭐⭐           |
| 考试出现率 | ⭐⭐⭐⭐⭐       | ⭐⭐⭐          |
| 代表    | ElastiCache | DAX          |

---

## 三、10 道 Amazon ElastiCache 高频考题（秒选）

### Q1

**ElastiCache 的核心作用是？**
✅ 减少数据库负载、降低延迟

---

### Q2

**ElastiCache 支持哪些引擎？**
✅ Redis、Memcached

---

### Q3

**需要持久化 + 排序 + Pub/Sub，用哪个？**
✅ Redis

---

### Q4

**亚毫秒级访问主要依赖什么？**
✅ 内存（in-memory）

---

### Q5

**Cache Miss 会发生什么？**
✅ 回源数据库并填充缓存

---

### Q6

**ElastiCache 是否替代数据库？**
❌ 否，只是缓存层

---

### Q7

**高并发读取 + 热点数据，首选？**
✅ ElastiCache

---

### Q8

**Redis Multi-AZ 的主要目的？**
✅ 高可用（failover）

---

### Q9

**缓存常见失效问题？**
✅ Cache stampede / hot key

---

### Q10

**读多写少、追求极低延迟？**
✅ ElastiCache

---

## 四、DynamoDB DAX vs ElastiCache（考试对比）

| 维度    | DynamoDB DAX  | Amazon ElastiCache |
| ----- | ------------- | ------------------ |
| 服务类型  | DynamoDB 专用缓存 | 通用缓存               |
| 模式    | Read-Through  | Cache Aside        |
| 支持 DB | 仅 DynamoDB    | 任意 DB              |
| 延迟    | 微秒级           | 亚毫秒                |
| 灵活性   | 低             | 高                  |
| 改代码   | 几乎不用          | 需要                 |
| 考试选择  | DynamoDB 专用优化 | 通用高性能缓存            |

📌 **考试金句**

> Use DAX to accelerate DynamoDB without changing application logic.
> Use ElastiCache for flexible, database-agnostic caching.

---

## 五、终极考试总结（背这一段就够）

> **ElastiCache is an in-memory cache used with the Cache Aside pattern to reduce database load and latency, while DynamoDB DAX provides a read-through cache specifically for DynamoDB.**

---

### 下一步你可以选（直接回数字）👇

1️⃣ 画 **Cache Stampede / Hot Key / TTL 失效** 时序图
2️⃣ Redis vs Memcached（SAA 必考对比）
3️⃣ Cache + ALB + RDS 三层架构完整串图
4️⃣ 出一套 **缓存陷阱题（高频误选）**

你选哪一个？
