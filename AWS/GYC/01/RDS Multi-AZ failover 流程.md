下面是**考试口径的一句话版 Step 1**（可直接背、可直接默念）：

> **Do not expose instances directly to the Internet; place an Application Load Balancer in public subnets across at least two Availability Zones to terminate HTTPS (TLS), serve as a single entry point, and integrate AWS WAF to protect backend resources from common web attacks such as SQL injection.**

如果你点头，我可以**用同样“一句话=一层架构”的方式**继续给你补全整套：

* **Step 2（一句话）**：Compute 层如何放在私有子网 + Auto Scaling
* **Step 3（一句话）**：Data 层为什么选 RDS Multi-AZ + Cache
* **Step 4（一句话）**：考试常见成本优化组合怎么选
* 🔧 最后 **汇总成一张完整三层架构 ASCII 图（考场脑内图）**

你只需要回一句：
👉 **继续 Step 2**


## 1) RDS Multi-AZ **failover 流程 ASCII 图（考试口径）**

### 正常状态（同步复制）

```
             (writes)
App  ----------------------->  Primary (AZ-a)
                                   |
                                   |  synchronous replication
                                   v
                              Standby (AZ-b)
                          (not serving reads/writes)
```

### 故障发生 → 自动切换（failover）

```
1) Primary in AZ-a fails
   App  ----X---->  Primary (down)

2) RDS promotes Standby to new Primary
   Standby (AZ-b)  --->  New Primary (AZ-b)

3) DNS / endpoint is updated (same endpoint)
   App  ----->  RDS Endpoint  ----->  New Primary
```

📌 **考试关键词**

* **Automatic failover**
* **Synchronous replication**
* **Standby does not serve traffic**
* **Same endpoint (DNS switchover)**

---

## 2) Multi-AZ vs Read Replica（考试必混对比）

| 维度            | RDS Multi-AZ                            | Read Replica                                 |
| ------------- | --------------------------------------- | -------------------------------------------- |
| 目的            | **高可用 / DR**                            | **读扩展 / 性能**                                 |
| 复制            | **同步 (sync)**                           | **异步 (async)**                               |
| 是否自动 failover | ✅ **是**                                 | ❌（通常不是“自动故障切换到 RR”）                          |
| 是否可读          | ❌ Standby 不对外读                          | ✅ Replica 可读                                 |
| 常见触发词         | **HA / automatic failover / resilient** | **read-heavy / scale reads / offload reads** |
| 成本            | 较高（多一份实例）                               | 取决于副本数量                                      |

📌 **一句话背诵**

* **Multi-AZ = availability**
* **Read Replica = performance (reads)**

---

## 3) 5 道 automatic failover 高频陷阱题（带答案+为什么坑）

### Q1

题干：**Relational DB requires automatic failover across AZs.**
选项里有：Read Replica / Multi-AZ / bigger instance / ElastiCache
✅ **答案：Multi-AZ**
🪤 坑点：Read Replica 不是为 failover 设计的主方案。

---

### Q2

题干：**Database performance is slow due to heavy read traffic; also needs high availability.**
✅ **答案：Multi-AZ + Read Replica**（两个都要）
🪤 坑点：只选 Multi-AZ 会错（它不提升读性能）。

---

### Q3

题干：**Enable Multi-AZ to improve read throughput.**（判断对错）
❌ **错**
🪤 坑点：Multi-AZ 的 standby 不提供读流量。

---

### Q4

题干：**An application connects to RDS using a fixed IP. During failover, it breaks. Best fix?**
✅ **答案：Use the RDS endpoint (DNS), not a fixed IP**
🪤 坑点：考点是 **endpoint/DNS switchover**。

---

### Q5

题干：**Need automatic failover with minimal app changes. Which is most appropriate?**
✅ **答案：RDS Multi-AZ**
🪤 坑点：很多人会选“add a replica”或“bigger EC2”，都不满足自动故障切换。

---

如果你愿意，我可以下一步把这块做成 **“一句话秒选卡”**（看到关键词就选 Multi-AZ / Read Replica / Cache），再加 **10 道混合陷阱题** 专门练你最容易混的点。




