下面给你一整套 **👉 可直接复制到 GitHub 的 Markdown 内容**，专门解决 **AWS 考试最容易混的点：Zonal vs Regional**。
结构清晰、**秒选导向**。

---

# 🧠 AWS 服务 × 隔离级别速记表（Exam Ready）

> 目标：**看到服务名，立刻知道是 Zonal / Regional / Global**

---

## 一、AWS 服务隔离级别总览表（核心必背）

### 🟦 Zonal（单 AZ 级别）

> **资源存在于一个 Availability Zone**

| 服务                      | 隔离级别  | 说明                  |
| ----------------------- | ----- | ------------------- |
| **EC2 Instance**        | Zonal | 实例只在一个 AZ           |
| **EBS Volume**          | Zonal | 只能挂同 AZ 的 EC2       |
| **Subnet**              | Zonal | 子网属于一个 AZ           |
| **NAT Gateway**         | Zonal | 每个 AZ 一个            |
| **EC2 Auto Scaling 实例** | Zonal | ASG 跨 AZ，但实例是 Zonal |

📌 **记忆法**：
👉 **“跟物理机器强绑定的，几乎都是 Zonal”**

---

### 🟩 Regional（跨 AZ 自动）

> **AWS 在 Region 内帮你处理 AZ 级容灾**

| 服务                 | 隔离级别     | 说明          |
| ------------------ | -------- | ----------- |
| **S3 Bucket**      | Regional | 自动跨 AZ      |
| **DynamoDB**       | Regional | 自动多 AZ      |
| **RDS Multi-AZ**   | Regional | 同步主备        |
| **ALB / NLB**      | Regional | 跨多个 AZ      |
| **VPC**            | Regional | 覆盖整个 Region |
| **Security Group** | Regional | 不属于某个 AZ    |
| **Route Table**    | Regional | 作用于子网       |

📌 **记忆法**：
👉 **“托管存储 / 网络 / LB，大多是 Regional”**

---

### 🟨 Global（不属于任何 Region）

> **全球服务，不受 Region 限制**

| 服务             | 隔离级别   | 说明             |
| -------------- | ------ | -------------- |
| **IAM**        | Global | 用户/角色全局        |
| **Route 53**   | Global | DNS            |
| **CloudFront** | Global | Edge Locations |
| **AWS Shield** | Global | DDoS 防护        |

📌 **记忆法**：
👉 **“身份、DNS、CDN = Global”**

---

## 二、🧠 一页「关键词 → 隔离级别」反射表

| 关键词                            | 秒反应      |
| ------------------------------ | -------- |
| **single AZ failure**          | Zonal    |
| **automatic multi-AZ**         | Regional |
| **physically attached**        | Zonal    |
| **high availability built-in** | Regional |
| **global users / CDN**         | Global   |
| **identity / access**          | Global   |

---

## 三、🧪 Zonal vs Regional 高频考试陷阱题（5 道）

---

### ❌ 陷阱题 1

**题目**：

> Which service is automatically replicated across multiple AZs?

❌ 错误选项：

* EBS Volume

✅ 正确答案：
👉 **S3**

📌 原因：

* EBS = Zonal
* S3 = Regional

---

### ❌ 陷阱题 2

**题目**：

> An EC2 instance fails due to an AZ outage.

❌ 错误理解：

* EC2 is Regional

✅ 正确认知：
👉 **EC2 instances are Zonal**

📌 原因：

* 实例只活在一个 AZ

---

### ❌ 陷阱题 3

**题目**：

> Which component must be created per AZ for outbound internet access?

❌ 错误选项：

* Internet Gateway

✅ 正确答案：
👉 **NAT Gateway**

📌 原因：

* IGW = Regional
* NAT Gateway = Zonal

---

### ❌ 陷阱题 4

**题目**：

> Which service remains available even if an AZ goes down?

❌ 错误选项：

* EBS

✅ 正确答案：
👉 **DynamoDB**

📌 原因：

* DynamoDB = Regional, multi-AZ

---

### ❌ 陷阱题 5

**题目**：

> Where should IAM users be created?

❌ 错误选项：

* In each Region separately

✅ 正确答案：
👉 **IAM is Global**

---

## 四、考试“反直觉”总规则（必背）

```text
❗ EC2 ≠ 高可用（默认）
❗ Zonal 资源一挂就没
❗ Regional 服务自动跨 AZ
❗ Multi-AZ ≠ Multi-Region
❗ Global 服务不属于任何 Region
```

---

## 五、30 秒考前自检清单

* 服务是不是 **跟物理机器绑定**？
* 是否明确说 **automatic replication**？
* 问的是 **AZ 故障** 还是 **Region 故障**？
* 有没有提 **Global / DNS / CDN**？

---

## ✅ 一句话终极总结（考试 / 面试通用）

> AWS 服务根据隔离级别可分为 Zonal、Regional 和 Global，其中 Zonal 资源绑定单个可用区，Regional 服务在区域内自动跨 AZ 提供高可用性，而 Global 服务不受区域限制。

---

### 👉 下一步我可以继续给你（同样 Markdown 可复制）：

* 🔥 **Zonal vs Regional vs Global 终极决策树**
* 🧪 **10 道混合隔离级别地狱题**
* 🧠 **VPC 组件 × 隔离级别速记卡**

你直接选一句：
👉 **“做决策树” / “再来 10 道地狱题” / “VPC 速记卡”**
