太好了，这一组我直接给你 **「一整套可落地的考试工具包」**：
**一张决策树 + 一页真假速查表 + 排除法训练题 + 方法论推广模板**。
全部是 **SAA-C03 考场可直接用** 的版本。

---

# 一、📦 Storage 决策树（ASCII / Markdown 版）

> 目标：**看到需求 → 顺着树走 → 自动选服务**

```
START
 |
 |-- 是否需要共享文件系统（多台 EC2 同时挂载）？
 |        |-- YES → EFS
 |        |-- NO
 |
 |-- 是否需要操作系统级磁盘（低延迟块设备）？
 |        |-- YES → EBS
 |        |-- NO
 |
 |-- 是否是对象存储（图片 / 视频 / 备份 / 日志）？
 |        |-- YES → S3
 |        |        |
 |        |        |-- 是否需要极低成本归档？
 |        |               |-- YES → S3 Glacier / Deep Archive
 |        |               |-- NO → S3 Standard / IA
 |        |
 |        |-- NO
 |
 |-- 是否是临时高速存储（实例生命周期内）？
 |        |-- YES → Instance Store
 |        |-- NO → ❌ 重新读题（八成是 S3）
```

### 🎯 决策树口诀（必背）

```
共享 → EFS
磁盘 → EBS
对象 → S3
归档 → Glacier
临时 → Instance Store
```

---

# 二、🧠 AWS 服务「真假速查表」（一页识破陷阱）

## 💾 Storage

| 说法                  | 真 / 假 | 考试判定           |
| ------------------- | ----- | -------------- |
| S3 是文件系统            | ❌ 假   | Object Storage |
| EFS 可多 EC2 挂载       | ✅ 真   | NFS            |
| EBS 可跨 AZ 挂载        | ❌ 假   | AZ 级           |
| Glacier 可实时访问       | ❌ 假   | 分钟~小时          |
| Instance Store 可持久化 | ❌ 假   | 随实例消失          |

---

## ⚙️ Compute

| 说法           | 真 / 假 | 考试判定     |
| ------------ | ----- | -------- |
| 单 EC2 可高可用   | ❌ 假   | 单点       |
| ASG 自动扩展     | ✅ 真   | Scalable |
| Lambda 适合长任务 | ❌ 假   | 时间限制     |
| ALB 支持 HTTPS | ✅ 真   | L7       |

---

## 🗄️ Database

| 说法                 | 真 / 假 | 考试判定          |
| ------------------ | ----- | ------------- |
| Multi-AZ 提升性能      | ❌ 假   | 只做 HA         |
| Read Replica 提升读性能 | ✅ 真   | Scale reads   |
| DAX 用于 RDS         | ❌ 假   | DynamoDB only |
| ElastiCache 可替代 DB | ❌ 假   | Cache only    |

---

## 🌐 Networking

| 说法                        | 真 / 假 | 考试判定          |
| ------------------------- | ----- | ------------- |
| Public IP = Public Subnet | ❌ 假   | 看 Route Table |
| NAT 支持入站                  | ❌ 假   | 出站 only       |
| SG 是 Stateful             | ✅ 真   | 自动回包          |
| NACL 有 Deny               | ✅ 真   | Stateless     |

---

# 三、🧪 10 道「排除法专用」训练题（只练删选项）

> 规则：**不选答案，先删错的**

---

### Q1

**Multiple EC2 instances need to share files**

* A. S3 ❌（不是文件系统）
* B. EBS ❌（不能多挂）
* C. **EFS ✅**
* D. Instance Store ❌（临时）

👉 **删到只剩 EFS**

---

### Q2

**Unpredictable traffic spikes, minimal ops**

* A. Single EC2 ❌
* B. Bigger EC2 ❌
* C. **ALB + ASG ✅**
* D. On-prem scaling ❌

---

### Q3

**Private subnet needs Internet access**

* A. IGW ❌（直连公网）
* B. Elastic IP ❌
* C. **NAT Gateway ✅**
* D. VPC Peering ❌

---

### Q4

**Relational DB, automatic failover**

* A. EC2 + MySQL ❌
* B. DynamoDB ❌
* C. **RDS Multi-AZ ✅**
* D. S3 ❌

---

### Q5

**Read-heavy workload, performance issue**

* A. Multi-AZ ❌
* B. Bigger DB ❌（不最优）
* C. **Read Replica / Cache ✅**
* D. Public Subnet ❌

---

### Q6

**Store images cheaply and durably**

* A. EBS ❌
* B. EFS ❌
* C. **S3 ✅**
* D. Instance Store ❌

---

### Q7

**Temporary credentials for EC2**

* A. Access Key ❌
* B. IAM User ❌
* C. **IAM Role ✅**
* D. Root ❌

---

### Q8

**Block specific IP ranges**

* A. Security Group ❌（无 Deny）
* B. **NACL ✅**
* C. Route Table ❌
* D. IGW ❌

---

### Q9

**Ultra-low latency cache**

* A. RDS ❌
* B. DynamoDB ❌
* C. **ElastiCache ✅**
* D. S3 ❌

---

### Q10

**Long-running background job**

* A. Lambda ❌
* B. **EC2 / ECS ✅**
* C. SQS ❌（只是队列）
* D. CloudFront ❌

---

# 四、🔁 把方法推广到 Compute / Database / Networking

## ✅ 通用「排除法模板」（任何服务都能用）

### Step 1：抓硬性限制

```
- 时间限制？
- 状态？
- 是否需要 HA？
- 是否需要共享？
```

---

### Step 2：删“天然不满足”的服务

```
- 单 EC2 → 删（HA / Scale）
- Lambda → 删（长任务 / 状态）
- S3 → 删（文件系统）
- Multi-AZ → 删（性能）
```

---

### Step 3：剩下的就是答案

> **SAA 的正确答案，往往是“唯一没被删掉的那个”**

---

## 🧠 套用到三大领域

### ⚙️ Compute

```
Long task? → 删 Lambda
Need HA? → 删单 EC2
Need scale? → 留 ASG
```

### 🗄️ Database

```
Need HA? → 删单实例
Need performance? → 删 Multi-AZ
Read-heavy? → 留 Read Replica / Cache
```

### 🌐 Networking

```
Inbound? → 删 NAT
Outbound only? → 删 IGW
Need deny? → 删 SG
```

---

# 五、终极一句话（考场用）

> **In SAA-C03, the fastest way to the correct answer is not picking the best option first, but eliminating the impossible ones.**

---

如果你愿意，下一步我可以：

* 🔥 做一套 **“只靠排除法就能做完”的 20 题模拟卷**
* 🧠 把 **Compute / DB / Network** 各做一张决策树
* 🎯 用你容易错的点，**定制一页“只留雷点”的速记卡**
* 📘 帮你把这整套内容整理成 **GitHub / Notion / PDF**

你想先继续 **哪一个？**
