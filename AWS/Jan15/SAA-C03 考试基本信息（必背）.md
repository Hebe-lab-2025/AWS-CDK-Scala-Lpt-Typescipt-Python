下面给你 **SAA-C03（AWS Certified Solutions Architect – Associate）** 的**权威考试信息 + 一套真考风 sample**，按你刷题/模考最有用的方式来。

---

## ✅ SAA-C03 考试基本信息（必背）

| 项目       | 内容                          |
| -------- | --------------------------- |
| **考试代码** | **SAA-C03**                 |
| **题目总数** | **65 题**                    |
| **计分题数** | **50 题**（≈15 题不计分的试验题，无法区分） |
| **考试时长** | **130 分钟**                  |
| **题型**   | 单选 + 多选                     |
| **通过分数** | **720 / 1000**              |
| **考试语言** | 英文（可选中文界面）                  |
| **难度定位** | 架构选型 + 高可用 + 成本优化 + 安全      |

👉 **关键心法**：
你不是在“写最强架构”，而是在 **选“最符合题干约束 + 成本/运维最合理”的方案**。

---

## 🧠 考试内容权重（考场直觉）

| Domain                        | 占比   | 考场体感                     |
| ----------------------------- | ---- | ------------------------ |
| Secure Architectures          | ~30% | IAM / KMS / VPC / Policy |
| Resilient Architectures       | ~26% | Multi-AZ / ASG / ALB     |
| High-Performing Architectures | ~24% | Cache / Scaling / LB     |
| Cost-Optimized Architectures  | ~20% | S3 Storage Class / Spot  |

---

## 🧪 SAA-C03 真考风 Sample（5 题）

### **Q1（LB 经典盲选）**

A company runs a web application that uses HTTP and requires path-based routing.
The solution must also integrate with AWS WAF.

Which load balancer should be used?

A. Network Load Balancer
B. Classic Load Balancer
C. **Application Load Balancer**
D. Gateway Load Balancer

✅ **答案：C**
🔑 关键词：**HTTP + path-based + WAF → ALB**

---

### **Q2（高可用 + 最小改动）**

An application runs on EC2 in a single Availability Zone.
The company wants **high availability** with **minimal changes**.

What should a Solutions Architect recommend?

A. Add EC2 instances in the same AZ
B. Move the app to Lambda
C. **Deploy EC2 instances in multiple AZs behind an ALB**
D. Use CloudFront

✅ **答案：C**
🔑 关键词：**HA + minimal change → Multi-AZ + ALB**

---

### **Q3（S3 成本陷阱）**

A company stores compliance data that is accessed once per year.
Data must be retained for 7 years at the **lowest cost**.

Which storage class should be used?

A. S3 Standard
B. S3 Intelligent-Tiering
C. **S3 Glacier Deep Archive**
D. S3 One Zone-IA

✅ **答案：C**
🔑 关键词：**very infrequent + long retention + cheapest → Deep Archive**

---

### **Q4（权限 AccessDenied 常考）**

An IAM policy allows `s3:GetObject`, but users still receive `AccessDenied`.

Which is the MOST likely cause?

A. Missing `s3:ListBucket`
B. **Bucket policy resource uses bucket ARN instead of object ARN**
C. IAM role is missing trust policy
D. S3 bucket is encrypted with SSE-S3

✅ **答案：B**
🔑 关键词：**GetObject → arn:aws:s3:::bucket/*（不是 bucket）**

---

### **Q5（缓存直觉题）**

A read-heavy application experiences high database load.
The solution must provide **sub-millisecond latency**.

Which service should be used?

A. Amazon RDS Read Replica
B. DynamoDB Global Table
C. **Amazon ElastiCache (Redis)**
D. Amazon Aurora Multi-AZ

✅ **答案：C**
🔑 关键词：**read-heavy + sub-ms → Redis**

---

## 🎯 考试节奏建议（65 题）

* **前 45 题**：平均 **~1.5 分钟/题**
* **后 20 题**：留 **30–35 分钟** 回看
* 多选题：**只要一个选项错，全题 0 分**

---

## 🔥 如果你要继续，我可以直接帮你：

1️⃣ 出 **一整套 65 题全真模考（90 分钟）**
2️⃣ 做 **“只看关键词秒选”暗号反射卷（LB / Cache / Storage）**
3️⃣ 给你 **SAA-C03 锁屏速记表 + Anki 卡**
4️⃣ 专刷 **反直觉错题（最容易 720→780 的那批）**

你直接说一句：**「来一套 65 题模考」** 或 **「只刷暗号题」** 即可。
