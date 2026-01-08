太好了，这一组我直接按 **SAA-C03 真题节奏**给你：
**题 → 决策 → 一页 Cheat Sheet**，全是 **秒选型**。

---

## 🔥 一、10 道 Aurora / RDS / Multi-AZ 场景题（100%真题风）

### 1️⃣

**需要 MySQL 兼容 + 高可用 + 自动故障切换**
👉 **RDS Multi-AZ**

---

### 2️⃣

**数据库宕机后必须自动切换，不关心读性能**
👉 **RDS Multi-AZ**
❌ Read Replica（只解决读）

---

### 3️⃣

**读请求很多，主库压力大**
👉 **RDS Read Replica**

---

### 4️⃣

**既要高可用，又要高读吞吐**
👉 **RDS Multi-AZ + Read Replicas**

---

### 5️⃣

**云原生关系型数据库，读写性能极高，自动扩展**
👉 **Aurora**

---

### 6️⃣

**需要 6 副本、跨 3 AZ、秒级故障恢复**
👉 **Aurora**

---

### 7️⃣

**现有 MySQL 应用，不想改代码，只要 HA**
👉 **RDS Multi-AZ**

---

### 8️⃣

**读多写少 + 报表分析**
👉 **RDS + Read Replicas**

---

### 9️⃣

**业务要求最小恢复时间（RTO）**
👉 **Aurora**

---

### 🔟

**“offload read traffic from primary database”**
👉 **Read Replica（不是 Multi-AZ）**

---

## 🧠 二、“看到关键词 → 秒选服务”决策表（考试核弹级）

| 看到的关键词                      | 秒选                        |
| --------------------------- | ------------------------- |
| high availability           | **Multi-AZ**              |
| automatic failover          | **Multi-AZ / Aurora**     |
| read-heavy                  | **Read Replica**          |
| offload read traffic        | **Read Replica**          |
| disaster recovery           | **Multi-AZ**              |
| performance                 | **Aurora / Read Replica** |
| cloud-native                | **Aurora**                |
| fastest failover            | **Aurora**                |
| MySQL/PostgreSQL compatible | **RDS / Aurora**          |
| no code change              | **RDS Multi-AZ**          |

### 🧠 考试口诀（一定要背）

> **Multi-AZ = 可用性**
> **Read Replica = 性能（读）**
> **Aurora = 高性能 + 高可用**

---

## 📄 三、一页 SAA-C03 数据库 Cheat Sheet（速记版）

### 🔹 RDS（基础款）

* 单实例 ❌ 不高可用
* 可加 **Multi-AZ / Read Replicas**

---

### 🔹 RDS Multi-AZ（必考）

* 同步复制
* 自动 failover
* **解决宕机，不解决读性能**

📌 关键词
`high availability` / `disaster recovery` / `automatic failover`

---

### 🔹 RDS Read Replica

* 异步复制
* **只读**
* 用于 **扩展读性能**

📌 关键词
`read-heavy` / `offload read traffic`

---

### 🔹 Aurora（高端款）

* 存储层自动 6 副本
* 跨 3 AZ
* **秒级 failover**
* 性能 > RDS

📌 关键词
`cloud-native` / `high performance` / `fast recovery`

---

## 🧠 最终「考试决策树」（闭眼用）

```
数据库宕机会不会影响业务？
        |
       是
        |
  是否需要极致性能？
     |        |
    否       是
     |        |
RDS Multi-AZ  Aurora

读请求很多？
     |
    是
     |
 Read Replica
```

---

## ✅ 考前 10 秒自检清单

* ❌ Multi-AZ ≠ 读扩展
* ❌ Read Replica ≠ 自动切换
* ✅ Aurora = 自带高可用 + 高性能
* ✅ 题干提 **offload read** → Read Replica

---

如果你愿意，下一步我可以直接给你：
🔥 **15 道 Aurora vs RDS 陷阱题**
🧪 **数据库选择“二选一”极限混淆题**
🧭 **SAA-C03 数据库完整决策脑图**

你说 **继续哪个** 👇
