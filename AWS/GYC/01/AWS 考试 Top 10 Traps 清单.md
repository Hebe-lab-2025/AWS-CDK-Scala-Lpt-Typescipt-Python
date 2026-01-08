下面这份是 **SAA-C03 / 面试通用的「Top 10 AWS 考试陷阱（Traps）」终极清单**。
目标：**看到触发词 → 立刻避坑 → 秒选正确答案**。

---

# 一、AWS 考试 **Top 10 Traps 清单（必背）**

## 🪤 Trap 1：**单台 EC2 万能论**

* ❌ 错觉：EC2 能解决一切
* ✅ 真相：**EC2 本身 ≠ 高可用 ≠ 自动扩展**
* 📌 正解：ALB + ASG / 托管服务

---

## 🪤 Trap 2：**Public IP = Public Subnet**

* ❌ 错觉：有 Public IP 就能上网
* ✅ 真相：**Subnet 是否 Public 只看 Route Table**
* 📌 正解：0.0.0.0/0 → IGW

---

## 🪤 Trap 3：**Private Subnet 不能访问 Internet**

* ❌ 错觉：私网就完全没网
* ✅ 真相：**NAT Gateway = 私网出网**
* 📌 正解：Private Subnet → NAT → IGW

---

## 🪤 Trap 4：**Security Group 能 Deny**

* ❌ 错觉：SG 和防火墙一样
* ✅ 真相：**SG 只能 Allow**
* 📌 正解：需要 Deny → 用 NACL

---

## 🪤 Trap 5：**RDS 一定要放 Public Subnet 才能连**

* ❌ 错觉：Private 连不上
* ✅ 真相：**RDS 推荐永远放 Private**
* 📌 正解：靠 SG，不靠公网

---

## 🪤 Trap 6：**Multi-AZ = 性能提升**

* ❌ 错觉：Multi-AZ 能分担读
* ✅ 真相：**Multi-AZ 只为高可用**
* 📌 正解：要性能 → Read Replica / Cache

---

## 🪤 Trap 7：**Lambda 适合所有计算**

* ❌ 错觉：Serverless = 万能
* ✅ 真相：**Lambda 有时间/状态限制**
* 📌 正解：长任务 → EC2 / ECS

---

## 🪤 Trap 8：**S3 是文件系统**

* ❌ 错觉：S3 像 NFS
* ✅ 真相：**S3 是 Object Storage**
* 📌 正解：共享文件 → EFS

---

## 🪤 Trap 9：**NAT Gateway 用来入站**

* ❌ 错觉：NAT = 网络万能
* ✅ 真相：**NAT 只负责出站**
* 📌 正解：入站靠 ALB / IGW

---

## 🪤 Trap 10：**CPU 100% = 需要更大实例**

* ❌ 错觉：直接升配
* ✅ 真相：**先查 SQL / 连接数 / 架构**
* 📌 正解：索引 / 连接池 / Cache / Read Replica

---

# 二、Trap + Trigger Words 对照表（条件反射版）

| Trigger Words（题干暗号）                | 常见陷阱           | 秒选正确方向               |
| ---------------------------------- | -------------- | -------------------- |
| **unpredictable traffic**          | 单 EC2          | ALB + ASG            |
| **high availability**              | 单 AZ           | Multi-AZ             |
| **private subnet internet access** | IGW            | NAT Gateway          |
| **block specific IPs**             | Security Group | NACL                 |
| **read performance issue**         | Multi-AZ       | Read Replica / Cache |
| **shared storage**                 | S3             | EFS                  |
| **long-running process**           | Lambda         | EC2 / ECS            |
| **DynamoDB slow reads**            | ElastiCache    | DAX                  |
| **database not accessible**        | NAT            | Security Group       |
| **minimal ops overhead**           | 自建方案           | 托管服务                 |

---

# 三、🧠「故意挖坑」模拟题（练直觉）

### 🧪 题 1（EC2 陷阱）

> A web application experiences unpredictable traffic spikes and must be highly available.

A. Use a single EC2 instance
B. Use EC2 with Elastic IP
C. **Use ALB with Auto Scaling** ✅
D. Increase EC2 instance size

📌 **挖坑点**：EC2 / 升配
📌 **触发词**：unpredictable + highly available

---

### 🧪 题 2（Subnet 陷阱）

> An EC2 instance has a public IP but cannot access the Internet.

A. Missing NAT Gateway
B. **Subnet route table missing IGW route** ✅
C. Security Group outbound rule
D. NACL is stateless

📌 **挖坑点**：Public IP
📌 **真因**：Route Table

---

### 🧪 题 3（RDS 陷阱）

> An application cannot connect to an RDS instance.

A. Add NAT Gateway
B. Make RDS public
C. **Allow EC2 security group in RDS SG** ✅
D. Use Elastic IP

📌 **挖坑点**：NAT / Public
📌 **核心**：SG → SG

---

### 🧪 题 4（性能陷阱）

> RDS CPU utilization reaches 100% during peak hours.

A. Enable Multi-AZ
B. Increase storage
C. **Add read replicas or caching** ✅
D. Move RDS to public subnet

📌 **挖坑点**：Multi-AZ
📌 **关键词**：performance / peak

---

### 🧪 题 5（存储陷阱）

> Multiple EC2 instances need to share files.

A. S3
B. EBS
C. **EFS** ✅
D. Instance Store

📌 **挖坑点**：S3
📌 **关键词**：shared filesystem

---

# 四、考试终极避坑口诀（一定要背）

```
Scalable ≠ EC2
Public ≠ Public IP
HA ≠ Performance
Private ≠ No Internet
Multi-AZ ≠ Faster
```

---

如果你愿意，下一步我可以：

* 🔥 再来 **10 道“连环陷阱题”**
* 🧪 做一套 **只考陷阱的 SAA 模拟卷**
* 🧠 带你 **逐题拆“为什么另外 3 个一定错”**
* 🎯 模拟 **真实考试读题 → 排雷 → 秒选全过程**

你选哪个？
