下面这组是 **「读压力专项」**，完全按 **SAA-C03 考试口径**来，保证你能 **一眼分清该用谁**。

---

## 一、📈 用 ASCII 图画 **读写分离（Read/Write Split）**

```
                    Reads (SELECT)
App  ------------------------------------+
 |                                        |
 | Writes (INSERT / UPDATE)               v
 v                                   Read Replica(s)
Primary DB (Writer)                  (Read-only)
   |                                      |
   |  synchronous (Multi-AZ)              |
   v                                      |
Standby (HA only)                         |
                                          |
                 <---- Application routes reads ---->
```

### 🔑 考试必须会的 3 点

* **Writes → Primary**
* **Reads → Read Replicas**
* **Standby（Multi-AZ）不参与读写**

📌 一句话记忆

> **Read replicas serve reads; Multi-AZ standby serves availability.**

---

## 二、Read Replica vs ElastiCache（读优化谁更合适？）

> 这是 **SAA 最容易混** 的一组，一定要分清 **“读扩展” vs “读加速”**

### 对比表（考试直选）

| 维度        | Read Replica            | ElastiCache                |
| --------- | ----------------------- | -------------------------- |
| 本质        | 数据库副本                   | 内存缓存                       |
| 解决问题      | **读扩展（Scale reads）**    | **读加速（Latency）**           |
| 延迟        | 毫秒级                     | **亚毫秒级**                   |
| 数据一致性     | 最终一致                    | 可能过期                       |
| 查询能力      | 完整 SQL                  | Key-Value                  |
| 典型触发词     | **read-heavy workload** | **low latency / hot data** |
| 是否减主库 CPU | 部分                      | **大幅**                     |

📌 **考试金句**

* **Read Replica = throughput**
* **ElastiCache = latency**

---

### 什么时候选谁？（条件反射）

* 题干出现 **“many SELECT queries” / “read-heavy”**
  👉 **Read Replica**
* 题干出现 **“sub-millisecond latency” / “frequently accessed data”**
  👉 **ElastiCache**
* 两个都出现？
  👉 **Read Replica + ElastiCache**

---

## 三、🧪 5 道「读压力陷阱题」（含避坑点）

### Q1

**RDS is under heavy read traffic and CPU utilization is high. What is the BEST solution?**

A. Enable Multi-AZ
B. Increase instance size
C. **Add Read Replicas** ✅
D. Move RDS to public subnet

🪤 坑点：Multi-AZ
📌 原因：Multi-AZ 不提升读性能

---

### Q2

**The application requires sub-millisecond response time for frequently accessed data.**

A. Read Replica
B. **ElastiCache** ✅
C. Multi-AZ
D. Larger DB instance

🪤 坑点：Read Replica
📌 原因：Replica 仍是数据库级延迟

---

### Q3

**Which solution reduces read latency AND offloads the primary database?**

A. Multi-AZ
B. Read Replica only
C. **ElastiCache** ✅
D. Increase storage

🪤 坑点：Read Replica only
📌 原因：Replica 分担读，但不保证极低延迟

---

### Q4

**An application uses Read Replicas but still experiences high latency on repeated queries. Best improvement?**

A. Add more replicas
B. **Add ElastiCache in front of DB** ✅
C. Enable Multi-AZ
D. Increase instance size

🪤 坑点：更多 Replica
📌 原因：热点数据更适合 Cache

---

### Q5

**Which statement is TRUE?**

A. Multi-AZ improves read throughput
B. **Read Replicas can serve read traffic** ✅
C. ElastiCache provides strong consistency
D. Standby instance handles SELECT queries

🪤 坑点：A / D
📌 原因：Standby 不对外服务

---

## 四、考试终极总结（直接背）

> **Use Read Replicas to scale read throughput, and use ElastiCache to reduce read latency by serving hot data from memory.**

---

如果你愿意，下一步我可以继续帮你把 **“读写分离 + Cache + Multi-AZ”** 串成一张 **完整数据层决策图**，或者出一套 **“读压力 vs 写压力” 混合陷阱题** 专门练反应速度。
