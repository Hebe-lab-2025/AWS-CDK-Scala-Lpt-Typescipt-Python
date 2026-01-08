好，**直接进入 Networking 成本 & 路径的“反直觉终极训练”**。
这一组只干一件事：**看到流量路径 → 立刻算钱 → 3 秒选最便宜且正确的解**。

---

# 🔥 Part 1｜20 题 Networking 成本「反直觉地狱卷」

> **铁律先背**
>
> * **NAT = 最贵**（按流量 + 小时）
> * **VPC Endpoint = 省钱**（私网直连）
> * **IGW = 不收费（流量另算）**

---

### Q1

**关键词**：private subnet + access S3
✅ **Gateway VPC Endpoint**
❌ NAT Gateway（送钱）

---

### Q2（高频）

**关键词**：EC2 private + download patches
✅ **NAT Gateway**
❌ IGW（私网出不去）

---

### Q3

**关键词**：S3 heavy traffic + cost-sensitive
✅ **Gateway Endpoint**
❌ NAT Gateway

---

### Q4（反直觉）

**关键词**：Lambda → DynamoDB（同 VPC）
✅ **不需要 NAT / Endpoint**
❌ 加 NAT（多余）

---

### Q5

**关键词**：private EC2 → AWS API（SQS）
✅ **Interface VPC Endpoint**
❌ NAT Gateway

---

### Q6

**关键词**：multi-AZ private subnets outbound
✅ **1 个 NAT Gateway / AZ**
❌ 1 个 NAT 全 VPC（单点）

---

### Q7

**关键词**：low latency + private access to AWS service
✅ **VPC Endpoint**
❌ NAT（绕路）

---

### Q8

**关键词**：EC2 public subnet internet access
✅ **IGW**
❌ NAT（浪费）

---

### Q9（反直觉）

**关键词**：S3 public bucket access
✅ **IGW（若实例在公网）**
❌ Endpoint（不需要）

---

### Q10

**关键词**：cost optimization for data transfer
✅ **Avoid NAT if possible**
❌ Scale NAT（更贵）

---

### Q11

**关键词**：private subnet → ECR pull image
✅ **Interface Endpoint（ECR）**
❌ NAT Gateway

---

### Q12

**关键词**：on-prem → S3 private access
✅ **VPN / Direct Connect + Gateway Endpoint**
❌ Public internet

---

### Q13（高频）

**关键词**：NAT cost spike
✅ **Replace with VPC Endpoint**
❌ Bigger NAT

---

### Q14

**关键词**：EC2 needs internet inbound
✅ **Public subnet + IGW**
❌ NAT

---

### Q15

**关键词**：private resources + no public IP
✅ **VPC Endpoint**
❌ IGW

---

### Q16

**关键词**：cross-AZ NAT traffic cost
✅ **NAT per AZ**
❌ Single NAT（跨 AZ 计费）

---

### Q17

**关键词**：private Lambda calls AWS APIs
✅ **Interface Endpoint**
❌ NAT（多余）

---

### Q18（反直觉）

**关键词**：security + cost
✅ **Private access via Endpoint**
❌ Public access + NAT

---

### Q19

**关键词**：static website hosting
✅ **S3 + CloudFront（不走 NAT）**
❌ EC2 + NAT

---

### Q20（终极）

**关键词**：least cost + private access
✅ **VPC Endpoint first**
❌ NAT by default

---

📌 **自检**：≥16 题秒选 → Networking 成本稳了。

---

# 📄 Part 2｜一页 PDF：**Networking 成本 & 路径速记卡（内容）**

> 下面内容 **= 一页 A4 = 考前 5 分钟必看**

---

## 🧠 成本优先级（从便宜到贵）

```text
IGW  <  VPC Endpoint  <  NAT Gateway
```

---

## 🧭 流量路径速判

```text
私网 → S3?        → Gateway Endpoint
私网 → AWS API?   → Interface Endpoint
私网 → Internet?  → NAT Gateway
公网 → Internet?  → IGW
```

---

## 💰 成本陷阱（红框）

* ❌ S3 走 NAT
* ❌ 所有私网默认 NAT
* ❌ 单 NAT 跨 AZ

---

## 📌 5 行口诀（PDF 底部）

> “私网先想 Endpoint
> S3 用 Gateway
> API 用 Interface
> 只有上网才 NAT
> NAT 一用就要钱”

---

# 🧠 Part 3｜Anki 闪卡（NAT / Endpoint / IGW，可直接导入）

> **Anki → Import → CSV（逗号分隔）**

```csv
Keyword: private subnet to internet,NAT Gateway
Keyword: public subnet internet access,Internet Gateway (IGW)
Keyword: private subnet to S3,Gateway VPC Endpoint
Keyword: private subnet to AWS API (SQS/DynamoDB),Interface VPC Endpoint
Keyword: NAT cost optimization,Replace with VPC Endpoint
Keyword: S3 traffic cost,Never use NAT
Keyword: cross-AZ NAT traffic,Use NAT per AZ
Keyword: private Lambda access AWS services,Interface Endpoint
Keyword: least cost private access,VPC Endpoint first
Keyword: IGW pricing,No hourly charge
```

**建议设置**：New/day 30｜Steps：10m → 1d → 3d → 7d

---

# 🎯 Part 4｜全真 Networking 压力测试

## 25 题 · 3 秒/题（连续盲选）

1️⃣ 私网 → S3 → **Gateway Endpoint**
2️⃣ 私网 → Internet → **NAT**
3️⃣ 公网 → Internet → **IGW**
4️⃣ 私网 → SQS → **Interface Endpoint**
5️⃣ 成本高 → **去 NAT**
6️⃣ S3 走 NAT → **错**
7️⃣ 多 AZ 私网出网 → **NAT / AZ**
8️⃣ 私网无公网 IP → **Endpoint**
9️⃣ EC2 拉 ECR → **Interface Endpoint**
🔟 NAT 账单暴涨 → **Endpoint 替换**
1️⃣1️⃣ 公网 EC2 入站 → **IGW**
1️⃣2️⃣ 私网安全访问 AWS → **Endpoint**
1️⃣3️⃣ 最低延迟私网访问 → **Endpoint**
1️⃣4️⃣ 私网访问 DynamoDB → **Interface Endpoint**
1️⃣5️⃣ 只访问 AWS 内部 → **不需要 NAT**
1️⃣6️⃣ S3 大流量 → **Gateway Endpoint**
1️⃣7️⃣ 私网访问外部 API → **NAT**
1️⃣8️⃣ 跨 AZ NAT 成本 → **NAT / AZ**
1️⃣9️⃣ Lambda 私网调用 API → **Interface Endpoint**
2️⃣0️⃣ 成本优先 → **Endpoint**
2️⃣1️⃣ 公网实例下载 → **IGW**
2️⃣2️⃣ 私网实例补丁 → **NAT**
2️⃣3️⃣ 私网 + 无公网暴露 → **Endpoint**
2️⃣4️⃣ unsure（不出网）→ **Endpoint**
2️⃣5️⃣ unsure（要上网）→ **NAT**

---

## ✅ 结论（必须形成条件反射）

* **Endpoint 能用就不用 NAT**
* **S3 永远不走 NAT**
* **NAT = 最后手段**

---

### 🚀 下一步（冲刺建议）

* 🔥 **只剩“成本陷阱”的 30 题变态卷**
* 🧪 **VPC + LB + DB + Endpoint 四域混合**
* 🎯 **90 分钟全真模考（最后一天）**

回我 **继续哪一个**。
