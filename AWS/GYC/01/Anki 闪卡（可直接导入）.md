太好了，这一轮我给你**“可直接用 + 高压实战”完整版**，不解释废话，**全部是拿分工具**。

---

## 🔥 Part 1｜Anki 闪卡（可直接导入）

> **使用方法**
> 1️⃣ 打开 Anki → Import
> 2️⃣ 粘贴下面内容 → 选择 **CSV / Tab 分隔**
> 3️⃣ Front = Question，Back = Answer

---

### 📌 Anki 卡组 ①：**DB 三选一（Aurora / DynamoDB / RDS）**

```csv
Keyword: sub-millisecond latency,Cache / DAX (not DB)
Keyword: key-value + massive scale,DynamoDB
Keyword: relational + joins,RDS / Aurora
Keyword: write-heavy relational,Aurora
Keyword: existing MySQL app + minimal change,RDS
Keyword: global read scalability,Aurora Global Database
Keyword: serverless DB + auto scale,DynamoDB
Keyword: HA relational (fast failover),Aurora
Keyword: read-heavy relational,Read Replica / Cache
Keyword: performance issue,Add Cache (not upgrade DB)
```

---

### 📌 Anki 卡组 ②：**缓存 & 高并发暗号**

```csv
Keyword: frequently accessed data,Cache
Keyword: hot keys,Cache / DAX
Keyword: burst traffic,Cache / Queue
Keyword: millions of requests per second,DynamoDB
Keyword: low latency reads,Cache
Keyword: DB overloaded,Cache + decouple
Keyword: scalability issue,NoSQL / Cache
Keyword: cost-effective performance,Cache
```

---

### 📌 Anki 卡组 ③：**考试反直觉必杀**

```csv
Keyword: performance slow,Do NOT upgrade DB first
Keyword: scalability problem,Do NOT use EC2
Keyword: HA issue,Aurora > RDS Multi-AZ
Keyword: read slow,Cache before DB
Keyword: write scale limit,RDS is not best
```

📌 **建议设置**：

* New cards/day：40
* Steps：10m → 1d → 3d → 7d
* 考前只刷 **Again / Hard**

---

## 🧪 Part 2｜三选一「地狱题」（全是干扰项）

> **规则**：
>
> * 每题 ≤ 3 秒
> * EC2 永远是干扰项
> * 我已帮你删掉“看起来很合理但一定错”的答案

---

### Q1

sub-ms latency + high read
A. RDS
B. Aurora
C. **Cache** ✅

---

### Q2

key-value + unpredictable traffic
A. RDS
B. **DynamoDB** ✅
C. EC2 + MySQL

---

### Q3

existing PostgreSQL app + HA
A. **RDS Multi-AZ** ✅
B. DynamoDB
C. EC2 + DB

---

### Q4

write-heavy + relational + scale
A. RDS
B. **Aurora** ✅
C. DynamoDB

---

### Q5

DB overloaded
A. Bigger DB
B. **Cache** ✅
C. EC2 ASG

---

### Q6

millions rps
A. Aurora
B. **DynamoDB** ✅
C. RDS

---

### Q7

read-heavy + relational
A. DynamoDB
B. **Read Replica / Cache** ✅
C. Bigger instance

---

### Q8

global reads + HA
A. RDS Multi-AZ
B. **Aurora Global DB** ✅
C. DynamoDB

---

### Q9

cost-sensitive + low ops
A. Aurora
B. **DynamoDB** ✅
C. Self-hosted DB

---

### Q10

performance issue (no more info)
A. Upgrade DB
B. **Add Cache** ✅
C. Change engine

---

📌 **如果你 ≥8 题秒选对 → 数据库域是稳分区**

---

## 📄 Part 3｜一页 PDF：**DB 扩展终极速记卡（内容）**

> 下面内容 **= 一页 A4 = 你考前最后 5 分钟只看它**

---

### 🧠 DB 三选一决策树（PDF 中央）

```text
数据模型？
 ├─ Key-Value ──> DynamoDB
 └─ Relational
       ├─ Write-heavy / HA ──> Aurora
       └─ Existing App ──> RDS
```

---

### ⚡ 性能优先级（必须背）

1️⃣ Cache
2️⃣ Read Replica
3️⃣ 换 DB（最后）

---

### 🚫 三大禁忌（红框）

* ❌ 性能慢 = 升 DB
* ❌ 高并发 = EC2 + DB
* ❌ 延迟高 = 换引擎

---

### 📌 5 行终极口诀（PDF 底部）

> “性能慢先加 Cache
> 模型不对换 DynamoDB
> 写多关系用 Aurora
> 旧系统用 RDS
> 不先升规格”

---

## 🎯 Part 4｜真实考试模拟：**连续 20 题强压秒选**

> **玩法**：
>
> * 连续看完 → 不停
> * 每题心里 3 秒
> * 最后对答案

---

### ① sub-ms latency → **Cache**

② key-value + scale → **DynamoDB**
③ relational + joins → **RDS / Aurora**
④ write-heavy relational → **Aurora**
⑤ DB overloaded → **Cache**
⑥ millions rps → **DynamoDB**
⑦ read-heavy → **Replica / Cache**
⑧ HA + fast failover → **Aurora**
⑨ performance issue → **Cache first**
⑩ cost-effective DB → **DynamoDB**
⑪ global reads → **Aurora Global DB**
⑫ schema-less → **DynamoDB**
⑬ existing MySQL → **RDS**
⑭ low latency → **Cache**
⑮ scalability issue → **NoSQL / Cache**
⑯ DB timeout → **Check network (not DB)**
⑰ hot keys → **Cache / DAX**
⑱ burst traffic → **Cache / Queue**
⑲ least ops → **Managed DB**
⑳ unsure → **Cache first**

---

## 🚀 接下来怎么冲（最有效顺序）

1️⃣ **Anki 连刷 2 轮（错卡 only）**
2️⃣ **③ 再来一套 65 题全真模考**
3️⃣ **最后 24 小时只刷“一页 PDF + 口诀”**

如果你愿意，我下一步可以：

* 🔥 给你 **Anki Deck 文件结构（.apkg 说明）**
* 🧪 再来一套 **“只剩 2 个答案”的变态卷**
* 🎯 模拟 **90 分钟全真考试**

你一句话告诉我：**继续哪一个**。
