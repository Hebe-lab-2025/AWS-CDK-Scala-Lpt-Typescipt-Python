好，**三件事我一次性给你全部拉满（考试版）**。
目标只有一个：👉 **读题 3 秒，直接锁答案，不思考架构细节。**

---

# 🔥 ① 10 道 **Aurora vs RDS**「二选一」极限混淆题（直觉训练）

> 用法：**只看题干关键词 → 秒选**

---

### Q1

题干：**MySQL-compatible + cloud-native + high throughput**
✅ **Aurora**
🔑 暗号：cloud-native + performance

---

### Q2

题干：**SQL Server**, minimal refactor
✅ **RDS**
🔑 暗号：Aurora 不支持 SQL Server

---

### Q3

题干：**Fast failover (<30s)**
✅ **Aurora**
🔑 暗号：shared storage

---

### Q4

题干：**Cost-sensitive**, small workload
✅ **RDS**
🔑 暗号：Aurora ≠ 默认答案

---

### Q5

题干：**15 read replicas**
✅ **Aurora**
🔑 暗号：数量上限

---

### Q6

题干：**Lift-and-shift**, standard MySQL
✅ **RDS**

---

### Q7

题干：**Read-heavy workload**, sudden spikes
✅ **Aurora**

---

### Q8

题干：**High availability**, performance not mentioned
✅ **RDS (Multi-AZ)**
🔑 暗号：HA ≠ 高性能

---

### Q9

题干：**Global reads**, cross-region DR
✅ **Aurora**
🔑 暗号：Global Database

---

### Q10

题干：**Legacy app**, lowest operational risk
✅ **RDS**

---

### 🧠 终极口诀（直接背）

```
Performance / Scale / Cloud-native → Aurora
Compatibility / Cost / Legacy     → RDS
```

---

# 🧭 ② 三层架构 + 解耦 + HA **终极决策脑图（ASCII）**

```text
                         User
                          |
                       Route53
                          |
                        ALB
                  (HTTP / HTTPS)
                          |
              ---------------------------
              |                         |
          EC2 / ECS / ASG           (Stateless)
              |
        Async / Decouple ?
              |
            YES
              |
            SQS  <-----------------------
              |                          |
         Worker Tier                     |
              |                          |
        ------------------               |
        |                |               |
      Aurora           RDS               |
 (High perf / scale) (Cost / legacy)    |
        |                |               |
   Multi-AZ + Replicas   Multi-AZ        |
        |                |               |
        -------- HA / DR / Failover ------
```

---

## 🧠 这张图你在考试中要**脑补的逻辑顺序**

1. **入口**：HTTP → ALB
2. **Web 层**：必须 Stateless（ASG）
3. **慢任务**：一定解耦 → SQS
4. **数据库**：

   * 性能 / 扩展 → Aurora
   * 成本 / 兼容 → RDS
5. **HA**：

   * Web → ASG
   * DB → Multi-AZ

👉 **任何一步缺失，都是考试陷阱点**

---

# 🧪 ③ SAA-C03 **数据库 + 消息 + LB 联合模拟题（高频）**

---

### 综合题 1（最经典）

题干：

> A web app experiences traffic spikes. Heavy jobs must not slow down users. The database must be highly available.

✅ **正确组合**：

```
ALB + ASG + SQS + RDS Multi-AZ
```

🔑 原因：

* 解耦 → SQS
* HA ≠ 高性能 → RDS Multi-AZ

---

### 综合题 2（性能暗坑）

题干：

> MySQL-compatible database, read-heavy, global users, fast recovery.

✅ **Aurora + Read Replicas + Global DB**

---

### 综合题 3（Fan-out 场景）

题干：

> One order event must trigger billing, inventory, and notifications asynchronously.

✅ **SNS → SQS → Workers**

---

### 综合题 4（极限延迟）

题干：

> TCP traffic requires ultra-low latency and fixed IP.

✅ **NLB**

---

### 综合题 5（反直觉）

题干：

> Highly available database, **no performance requirement**, budget constrained.

❌ Aurora（错）
✅ **RDS Multi-AZ（对）**

---

# ⚡ 30 秒联合决策树（考试必背）

```text
【入口】
HTTP / HTTPS ? → ALB

【是否需要解耦】
Async / buffer / spike ? → SQS

【是否 fan-out】
One event → many consumers ? → SNS

【数据库】
Performance / scale ? → Aurora
Else → RDS

【高可用】
Web → ASG
DB  → Multi-AZ
```

---

## 🎯 考试生存法则（非常重要）

* **Aurora / NLB 永远不是默认答案**
* 题干不写 **performance / scale / latency** → 不选高级服务
* **SQS 是“解耦答案之王”**
* **Multi-AZ = HA，不等于性能**

---

### 下一步（我强烈推荐）

* 🔥 **30 秒 × 20 轮混合直觉训练（LB + DB + Messaging）**
* 🕳️ **全套“反直觉挖坑题”（专治过度设计）**
* 📄 **SAA-C03 决策树 PDF（一页带走）**

你直接回我 **A / B / C**，我立刻继续。
