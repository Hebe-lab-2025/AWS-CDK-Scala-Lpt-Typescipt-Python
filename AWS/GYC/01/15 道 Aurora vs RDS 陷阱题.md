下面是 **SAA-C03 考试专用 · 数据库选择“秒杀版”材料**，目标只有一个：
👉 **看到题干 3 秒内锁定 Aurora 还是 RDS**

---

# 🔥 Part 1：15 道 **Aurora vs RDS** 陷阱题（真题风格）

> 使用方法：
> **先扫关键词 → 再看答案 → 背一句话理由**

---

### Q1

题干：需要 **MySQL 兼容 + 极高吞吐 + 云原生架构**
✅ **Aurora**
👉 云原生 + 高性能 = Aurora

---

### Q2

题干：公司已有 **Oracle / SQL Server** 数据库
✅ **RDS**
👉 Aurora **不支持 Oracle / SQL Server**

---

### Q3

题干：希望 **读写自动扩展，几乎不需要运维**
✅ **Aurora**
👉 Serverless / 自动存储扩展是 Aurora 强项

---

### Q4

题干：**成本敏感的小型应用**
✅ **RDS**
👉 Aurora 起步成本更高

---

### Q5

题干：需要 **15 个 Read Replicas**
✅ **Aurora**
👉 Aurora 支持 **最多 15 个副本**

---

### Q6

题干：传统应用，**不想改数据库引擎**
✅ **RDS**
👉 Lift-and-shift 场景 = RDS

---

### Q7

题干：**亚秒级故障恢复（<30s）**
✅ **Aurora**
👉 Aurora Failover 通常 10–30 秒

---

### Q8

题干：强调 **Multi-AZ 高可用，但性能不是瓶颈**
✅ **RDS**
👉 高可用 ≠ 高性能

---

### Q9

题干：**读流量暴增**，主库压力大
✅ **Aurora**
👉 Aurora 副本共享存储，读扩展更强

---

### Q10

题干：需要 **跨 Region 读副本（灾备）**
✅ **Aurora**
👉 Aurora Global Database

---

### Q11

题干：明确写了 **“Aurora-compatible MySQL”**
✅ **Aurora**
👉 这是明牌

---

### Q12

题干：需要 **最少改动 + 标准 MySQL**
✅ **RDS**

---

### Q13

题干：**云原生、微服务、快速扩展**
✅ **Aurora**

---

### Q14

题干：预算有限，但需要 **Multi-AZ**
✅ **RDS**
👉 RDS Multi-AZ 更便宜

---

### Q15

题干：**“Performance at scale”**
✅ **Aurora**
👉 考试暗号

---

## 🧠 一句话终极口诀

> **高性能 / 云原生 / 自动扩展 / 全球 → Aurora**
> **兼容性 / 低成本 / 传统系统 → RDS**

---

# 🧪 Part 2：数据库选择「二选一」极限混淆题

> 考试最容易错的三组👇

---

### ❌ 混淆点 1：**Multi-AZ ≠ 高性能**

| 需求  | 正确选择         |
| --- | ------------ |
| 高可用 | RDS Multi-AZ |
| 高性能 | Aurora       |

👉 Multi-AZ 是 **HA 方案，不是性能方案**

---

### ❌ 混淆点 2：**Read Replica 数量**

| 服务     | 副本   |
| ------ | ---- |
| RDS    | ≤ 5  |
| Aurora | ≤ 15 |

---

### ❌ 混淆点 3：**存储架构**

| 服务     | 存储      |
| ------ | ------- |
| RDS    | 实例级存储   |
| Aurora | 共享分布式存储 |

👉 共享存储 = 快速 failover + 高读性能

---

# 🧭 Part 3：SAA-C03 **数据库完整决策脑图（ASCII）**

```text
                 Database Choice
                        |
        ----------------------------------
        |                                |
   需要云原生？                     传统 / 成本优先？
        |                                |
      YES                              YES
        |                                |
    Aurora                           RDS
        |
  -------------------------
  |           |           |
 高性能    自动扩展     全球复制
```

---

## ⚡ 3 秒考试决策树（必背）

```text
【Step 1】是否支持 Oracle / SQL Server？
   └─ YES → RDS

【Step 2】是否强调 performance / scale / cloud-native？
   └─ YES → Aurora

【Step 3】是否成本敏感 + 小规模？
   └─ YES → RDS
```

---

## 🎯 考试生存法则（非常关键）

* **Aurora = 高级选项，不是默认选项**
* 题干没给“性能 / 扩展 / 云原生” → **不要选 Aurora**
* **RDS 是安全答案**

---

如果你愿意，我可以下一步直接给你👇

* 🔥 **20 道 Aurora vs DynamoDB vs RDS 三选一魔鬼题**
* 🧠 **SAA-C03 数据库“暗号词典”全集**
* 📄 **数据库章节 PDF（10 页，专刷）**
* 🧪 **数据库 × 缓存 × 高并发组合题**

你点一个，我马上继续。
