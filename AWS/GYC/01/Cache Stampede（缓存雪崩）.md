下面这套是 **SAA-C03 缓存域“必杀组合”**：
👉 **机制图 + 对比表 + 架构串图 + 陷阱题**（全部按“秒选”口径）

---

## 1️⃣ Cache Stampede / Hot Key / TTL 失效 —— **时序图（ASCII）**

### 🔥 A. Cache Stampede（缓存雪崩）

> **大量 key 同时过期 → DB 被瞬间打爆**

```text
Time →
Users  ──► Cache (MISS) ──► DB (QPS 暴涨 ❌)
Users  ──► Cache (MISS) ──► DB
Users  ──► Cache (MISS) ──► DB
           ↑
        同一时间 TTL 过期
```

**考试对策（看到就选）：**

* TTL **加随机抖动（jitter）**
* **预热 / 提前刷新**
* **Read-through / Write-through**

---

### 🔥 B. Hot Key（热点 Key）

> **一个 key 被极高并发反复访问**

```text
Users (10k QPS)
      └────────► Cache [HOT KEY]
                      │
                      └─ 单点 CPU/锁 争用 ❌
```

**考试对策：**

* **Key 拆分（sharding）**
* **本地缓存 + Redis**
* **副本 / 读扩展**

---

### 🔥 C. TTL 失效穿透（Cache Miss Storm）

> **请求命中不存在的数据**

```text
Users ──► Cache (MISS)
           │
           └─► DB (查不到)
Users ──► Cache (MISS)
           │
           └─► DB
```

**考试对策：**

* **缓存空值（Negative Cache）**
* **Bloom Filter**

---

## 2️⃣ Redis vs Memcached（**SAA 必考对比表**）

| 维度      | **Redis (ElastiCache)**           | **Memcached (ElastiCache)** |
| ------- | --------------------------------- | --------------------------- |
| 数据结构    | String / Hash / List / Set / ZSet | Key-Value                   |
| 持久化     | ✅ 支持                              | ❌ 不支持                       |
| 高可用     | ✅ Multi-AZ / Replicas             | ❌                           |
| Pub/Sub | ✅                                 | ❌                           |
| 排序 / 计数 | ✅（ZSet）                           | ❌                           |
| TTL     | ✅                                 | ✅                           |
| 用途      | **复杂缓存 / 会话 / 排行榜**               | **极简单对象缓存**                 |
| 考试默认    | **Redis**                         | 只有“simple cache”才选          |

**一句话秒选：**

```
需要结构 / HA / 排序 / 会话 → Redis
只要极简单 KV → Memcached
```

---

## 3️⃣ Cache + ALB + RDS —— **三层架构完整串图**

```text
Clients
   |
 Route53
   |
  ALB  (HTTPS / TLS terminate)
   |
 Stateless Web Tier (ASG)
   |
   | ① 先查缓存
   v
ElastiCache (Redis)
   |
   | ② Cache MISS
   v
RDS / Aurora (Multi-AZ)
   |
   | ③ 回写缓存（Write-through / Lazy）
   v
ElastiCache
```

### 🧠 考试必须脑补的 4 个点

1. **ALB**：入口 + TLS 终止
2. **Web 层**：Stateless（ASG）
3. **Cache**：**先查缓存**（读优化）
4. **DB**：**Multi-AZ = HA，不是性能**

---

## 4️⃣ 缓存陷阱题（**高频误选**）

### Q1

题干：**sub-millisecond latency** 读请求
✅ **ElastiCache (Redis)**
❌ RDS（错）

---

### Q2

题干：大量请求命中 **不存在的数据**
✅ **Negative cache / Bloom filter**
❌ 加大 DB（错）

---

### Q3

题干：排行榜 / Top-N
✅ **Redis ZSet**
❌ Memcached（错）

---

### Q4

题干：缓存节点宕机导致会话丢失
✅ **Redis + Replicas / Multi-AZ**
❌ Memcached（错）

---

### Q5

题干：缓存过期时 DB 被打爆
✅ **TTL jitter / preload**
❌ 提高 DB 规格（错）

---

### Q6

题干：简单对象缓存、最低成本
✅ **Memcached**
❌ Redis（过度设计）

---

### Q7

题干：Web 扩容后 session 丢失
✅ **Session 存 Redis**
❌ 本地内存（错）

---

### Q8

题干：读多写少、读压力大
✅ **Cache in front of DB**
❌ Read Replica 优先（不如缓存）

---

## 🧠 缓存终极速记卡（必背）

```
亚毫秒 → Cache
读多写少 → Cache
排行榜 / 计数 → Redis
简单 KV → Memcached

Cache 雪崩 → TTL 抖动
Cache 穿透 → 空值缓存 / Bloom
Hot Key → 拆分 / 本地缓存
```

---

## ⚡ 30 秒缓存决策树（闭眼版）

```text
是否需要极低延迟？
  └─ YES → Cache

是否需要结构 / HA / 排序？
  └─ YES → Redis

是否只是简单 KV？
  └─ YES → Memcached

DB 被打爆？
  └─ Cache + TTL 策略
```

---

## 🎯 考试生存法则

* **“亚毫秒” = 你不该打数据库**
* **Multi-AZ ≠ 性能**
* **Redis 是默认缓存答案**
* **缓存问题先想策略，再想扩容**

---

如果你愿意，下一步我可以继续给你👇

* 🔥 **Cache × DynamoDB × RDS 三选一魔鬼题**
* 🧠 **SAA-C03 性能优化“暗号词典”全集**
* 📄 **Caching Domain 一页 PDF（考前 5 分钟）**

回我 **A / B / C**，我立刻继续。
