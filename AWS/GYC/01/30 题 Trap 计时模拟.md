好，**A + B** 一次性给你，都是 **最提分形态** 👇
（不用理解，只要 **条件反射**）

---

# A️⃣ 数据层「3 秒决策图 + 口诀」

## 📄 一页 PDF + 🧠 Anki 闪卡

### ✅ 已生成 · 一页 PDF（考试专用）

**下载：**
👉 [SAA-C03_Data_Layer_Decision_OnePage.pdf](sandbox:/mnt/data/SAA-C03_Data_Layer_Decision_OnePage.pdf)

> 用法：
>
> * **考前 5 分钟** 扫一遍
> * 所有 DB 题只问 5 个问题：
>   **事务？读？写？延迟？跨区？**

---

### 🧠 Anki 闪卡（数据层版）

> 你已经有 Networking 版，这一组是 **Data Layer 专杀**

**正面 → 背面（示例）**

* `Need automatic failover (HA)?`
  → **RDS / Aurora Multi-AZ**

* `Read-heavy workload?`
  → **Read Replica / Aurora Reader**

* `sub-millisecond latency / hot keys?`
  → **ElastiCache / DynamoDB DAX**

* `High write throughput, no joins?`
  → **DynamoDB**

* `Low RPO cross-region DR?`
  → **Aurora Global Database**

如果你要：

* 我可以 **直接帮你合并进刚才那个 Anki Deck**
* 或给你 **单独一份 Data Layer Anki TSV**

---

# B️⃣ 20 题「读 / 写 / 延迟」地狱混合卷

**⏱️ 3 秒 / 题｜总时长 60 秒**

> **规则**：
> 不许分析，只看关键词 → 选答案

---

## Q1–Q20（只选 A/B/C/D）

1. 题干：**automatic failover**
   A Read Replica
   B Multi-AZ
   C Cache
   D DynamoDB

2. **read-heavy workload**
   A Multi-AZ
   B Bigger instance
   C Read Replica
   D Snapshot

3. **sub-millisecond latency**
   A RDS
   B DynamoDB
   C Cache/DAX
   D Aurora

4. **frequent writes, horizontal scale, no joins**
   A RDS
   B Aurora
   C DynamoDB
   D ElastiCache

5. **读写都高，但热点 Key 极集中**
   A Read Replica
   B Cache
   C Multi-AZ
   D Bigger DB

6. **high availability only（no perf issue）**
   A Read Replica
   B Multi-AZ
   C Cache
   D DynamoDB

7. **read latency spikes during traffic burst**
   A Bigger instance
   B Multi-AZ
   C Read Replica
   D Cache

8. **cross-region DR, RPO must be very low**
   A Snapshot
   B Multi-AZ
   C Aurora Global DB
   D Read Replica same region

9. **millions of requests, key-value access**
   A RDS
   B Aurora
   C DynamoDB
   D S3

10. **join + transaction required**
    A DynamoDB
    B ElastiCache
    C RDS/Aurora
    D S3

11. **read scaling without downtime**
    A Multi-AZ
    B Read Replica
    C Snapshot
    D Backup

12. **hot data cached in memory**
    A Read Replica
    B DAX
    C Multi-AZ
    D Snapshot

13. **write-heavy + predictable schema**
    A DynamoDB
    B Cache
    C RDS
    D S3

14. **global low-latency reads**
    A Single RDS
    B Read Replica cross-region
    C Cache only
    D Snapshot

15. **misconception check：Multi-AZ provides read scaling?**
    A True
    B False
    C Sometimes
    D Depends

16. **read traffic offload from primary DB**
    A Multi-AZ
    B Read Replica
    C Cache only
    D Backup

17. **millisecond-level latency acceptable, need SQL**
    A DynamoDB
    B Cache
    C RDS/Aurora
    D S3

18. **hot key workload on DynamoDB**
    A Read Replica
    B DAX
    C Multi-AZ
    D Bigger table

19. **DR but cost-sensitive, slower recovery ok**
    A Aurora Global
    B Cross-region snapshot
    C Multi-AZ
    D Cache

20. **考试看到 sub-ms + database**
    A Scale DB
    B Change engine
    C Add Cache
    D Multi-AZ

---

## ✅ 答案（条件反射版）

```
1B  2C  3C  4C  5B
6B  7D  8C  9C  10C
11B 12B 13C 14B 15B
16B 17C 18B 19B 20C
```

---

## 🎯 读写延迟终极口诀（背这个就够）

> **HA → Multi-AZ**
> **Read → Replica**
> **Latency → Cache**
> **Write scale → DynamoDB**
> **Global DR → Aurora Global**

---

### 下一步我可以直接帮你：

* 🔥 把 **A + B + Networking** 全部合并成
  **「SAA-C03 终极 Anki Deck（100+ 卡）」**
* ⏱️ 再来一轮 **30 题「数据层 + 网络混合 3 秒卷」**
* 📊 做一次 **完整 65 题 + 自动错因统计表**

你选一个，我直接继续。
