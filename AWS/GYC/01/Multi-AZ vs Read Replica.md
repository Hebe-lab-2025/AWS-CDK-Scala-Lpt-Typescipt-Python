太好了 👍
我直接**帮你把最核心、最容易考混的内容一次性讲清楚**，按**考试视角**来。

---

# 一、Multi-AZ vs Read Replica（ASCII 对比图）

## 1️⃣ Multi-AZ（高可用 / HA）

```
        App
         |
     RDS Endpoint
         |
   ┌─────────────┐
   │  Primary DB │  ← 写
   └──────┬──────┘
          │ 同步复制
   ┌──────▼──────┐
   │ Standby DB  │  ← 不接读
   └─────────────┘
     （另一个 AZ）
```

**考试结论：**

* ✅ **解决：高可用 / 自动故障切换**
* ❌ **不提升读性能**
* ❌ 不能手动读 standby

📌 **关键词命中**：

> *high availability / automatic failover*

---

## 2️⃣ Read Replica（性能 / 扩展）

```
            App
          /     \
     Read        Read
   Replica A   Replica B
        ▲         ▲
        └── 异步复制 ──┐
                       │
                 ┌─────▼─────┐
                 │ Primary DB │ ← 写
                 └───────────┘
```

**考试结论：**

* ✅ **解决：读性能 / 扩展**
* ❌ **不自动 failover**
* ❌ 复制是异步（可能延迟）

📌 **关键词命中**：

> *read-heavy / scale reads*

---

## 二、一句话“秒杀级对比”

| 维度   | Multi-AZ | Read Replica |
| ---- | -------- | ------------ |
| 目的   | **高可用**  | **性能**       |
| 复制   | 同步       | 异步           |
| 故障切换 | 自动       | 手动           |
| 能不能读 | ❌        | ✅            |
| 性能提升 | ❌        | ✅            |

👉 **HA ≠ 性能（考试最爱坑）**

---

# 三、5 道「HA vs 性能」陷阱题（含秒选答案）

### Q1

> Database must survive AZ failure automatically
> ✅ **Multi-AZ**

---

### Q2

> Application has slow SELECT queries under heavy traffic
> ✅ **Read Replica**

---

### Q3

> Improve availability with no code change
> ✅ **Multi-AZ**

---

### Q4

> Reduce load on primary DB for reporting queries
> ✅ **Read Replica**

---

### Q5

> Need both HA and read scaling
> ✅ **Multi-AZ + Read Replicas**

---

# 四、Aurora vs RDS（考试升级版）

## 1️⃣ 架构本质差异

### RDS（传统）

```
Compute + Storage
(绑在一起)
```

### Aurora（云原生）

```
Compute (DB Instance)
        |
Shared Distributed Storage
(6 copies, 3 AZ)
```

---

## 2️⃣ 核心对比表（必背）

| 维度       | RDS          | Aurora      |
| -------- | ------------ | ----------- |
| 存储       | 单实例          | **分布式存储**   |
| 副本       | Read Replica | **最多 15 个** |
| Failover | 分钟级          | **秒级**      |
| 性能       | 标准           | **更高**      |
| 成本       | 较低           | 较高          |

📌 **考试倾向**：

> 要 **高性能 + 高可用 + 读扩展** → **Aurora**

---

# 五、三层架构（考试最终形态，完整串起来）

```
──────── Presentation Layer ────────
User
 ↓
ALB (HTTPS / SSL)

──────── Application Layer ─────────
EC2 / ECS (Auto Scaling)

──────── Data Layer ────────────────
Aurora / RDS
 ├─ Multi-AZ (HA)
 └─ Read Replicas (性能)
```

📌 **铁律（考试）**：

* Web 层：Auto Scaling
* DB 层：Multi-AZ
* 读多：Read Replica
* 要顶配：Aurora

---

# 六、最终「一行决策口诀」

> **Multi-AZ 保命，Read Replica 提速，Aurora 两个都要**

---

如果你愿意，下一步我可以帮你做 **考试冲刺级内容** 👇

* 🔥 **10 道 Aurora / RDS / Multi-AZ 场景题（100%真题风）**
* 🧠 **“看到关键词 → 秒选服务”决策表**
* 📄 **一页 SAA-C03 数据库 Cheat Sheet**

你想继续 **刷题** 还是 **做速记表**？
