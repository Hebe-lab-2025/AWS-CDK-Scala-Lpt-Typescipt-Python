下面给你 **纯考试向、反直觉、高杀伤力** 的内容，目标只有一个：

> **看到关键词 → 3 秒内条件反射选答案**

---

# 🔥 Part 1：10 道 Networking「反直觉地狱题」（SAA-C03 风格）

---

## 1️⃣ 私有子网 EC2 访问 S3 很慢 + 成本高

**暗号**：`private subnet` + `S3 access` + `cost-sensitive`

❌ NAT Gateway
✅ **S3 Gateway Endpoint**

👉 **反直觉点**：
很多人觉得“出不去网 → NAT”，但 **访问 S3 不走公网**，用 Gateway Endpoint **免费 + 更快**。

---

## 2️⃣ 私有子网 EC2 拉 Docker 镜像失败

**暗号**：`ECR` + `private subnet` + `no internet`

❌ S3 Endpoint
❌ NAT Gateway（不是最优）
✅ **Interface Endpoint（ECR API + DKR）**

👉 **反直觉点**：
ECR 是 **API 服务**，不是 S3，对应 **Interface Endpoint**。

---

## 3️⃣ RDS 在私有子网，公网客户端连不上

**暗号**：`RDS` + `private subnet` + `external access`

❌ 给 RDS 加 Public IP
❌ NAT Gateway
✅ **Bastion Host / VPN / Direct Connect**

👉 **铁律**：
**NAT 只能出，不能进**

---

## 4️⃣ ALB 后端实例频繁超时

**暗号**：`ALB timeout` + `backend healthy`

❌ 调大 EC2
❌ 改实例类型
✅ **检查 Security Group（ALB → EC2）**

👉 **反直觉点**：
90% 是 **网络规则问题，不是算力问题**

---

## 5️⃣ 跨 AZ 流量费用突然暴涨

**暗号**：`cross-AZ traffic cost`

❌ NAT Gateway 多建几个
❌ 合并子网
✅ **开启 ALB Cross-Zone Load Balancing**

👉 **反直觉点**：
ALB 默认 **不均衡**，可能导致 **跨 AZ 回流**

---

## 6️⃣ Lambda 访问 RDS 超慢

**暗号**：`Lambda` + `VPC` + `slow DB`

❌ 加大 Lambda 内存
❌ 改 RDS 实例
✅ **检查是否在 VPC + 是否走 NAT**

👉 **反直觉点**：
Lambda 进 VPC = **失去 AWS 内部网络加速**

---

## 7️⃣ 私有子网实例无法访问 DynamoDB

**暗号**：`DynamoDB` + `private subnet`

❌ NAT Gateway
❌ Interface Endpoint
✅ **DynamoDB Gateway Endpoint**

👉 **记忆规则**：
**S3 / DynamoDB = Gateway Endpoint**

---

## 8️⃣ CloudFront 访问私有 ALB

**暗号**：`CloudFront` + `private ALB`

❌ 给 ALB Public IP
❌ NAT Gateway
✅ **ALB 必须是 Internet-facing**

👉 **反直觉点**：
CloudFront ≠ VPN，它仍然走公网。

---

## 9️⃣ EC2 只能访问 AWS 服务，不能访问互联网

**暗号**：`AWS services only` + `cost control`

❌ NAT Gateway
❌ Internet Gateway
✅ **VPC Endpoints（Gateway / Interface）**

👉 **考试潜台词**：
“不需要公网” = **不要 NAT**

---

## 🔟 同 VPC 不同子网 EC2 无法通信

**暗号**：`same VPC` + `cannot connect`

❌ Peering
❌ NAT
✅ **检查 NACL（默认拒绝）**

👉 **反直觉点**：
SG 是 **允许即放行**
NACL 是 **无状态 + 顺序匹配**

---

---

# 🧠 Part 2：SAA-C03 全域「3 秒决策总表」（最终版）

> **考试不是设计系统，是识别暗号**

---

## 🌐 网络类（Networking）

| 暗号                        | 3 秒结论                  |
| ------------------------- | ---------------------- |
| private subnet → AWS 服务   | **VPC Endpoint**       |
| S3 / DynamoDB             | **Gateway Endpoint**   |
| API / ECR / SSM           | **Interface Endpoint** |
| private subnet → internet | **NAT Gateway**        |
| inbound from internet     | **ALB / NLB（公网）**      |
| NAT 进站？                   | ❌ **不可能**              |
| 同 VPC 不通                  | **NACL / SG**          |

---

## 🗄️ 数据库类（DB）

| 暗号                  | 秒选                    |
| ------------------- | --------------------- |
| high availability   | **Multi-AZ**          |
| read heavy          | **Read Replica**      |
| sub-millisecond     | **ElastiCache / DAX** |
| unpredictable scale | **Aurora / DynamoDB** |

---

## ⚖️ 负载均衡（LB）

| 暗号              | 秒选            |
| --------------- | ------------- |
| HTTP / HTTPS    | **ALB**       |
| TCP / UDP       | **NLB**       |
| TLS termination | **ALB + ACM** |
| static IP       | **NLB**       |

---

## 🧱 解耦 & 扩展

| 暗号               | 秒选         |
| ---------------- | ---------- |
| async processing | **SQS**    |
| fan-out          | **SNS**    |
| buffer spikes    | **SQS**    |
| event-driven     | **Lambda** |

---

## 💸 成本陷阱

| 场景                | 正解 |
| ----------------- | -- |
| 访问 S3 却用 NAT      | ❌  |
| 只访问 AWS 服务却给公网    | ❌  |
| Lambda 进 VPC 却不需要 | ❌  |

---

## 🎯 一句话考试心法（请背）

> **AWS 考试不是“我会怎么设计”，
> 而是“AWS 已经给你准备了什么托管答案”。**

---

如果你愿意，我可以下一步直接帮你：

* 📄 生成 **《Networking + DB 决策树 一页 PDF》**
* 🧪 再出 **20 道“只看暗号盲选题”**
* 🧠 把这套内容压缩成 **考前 5 分钟速记卡**
* 🎯 做一轮 **90 分钟真实模拟 → 错题反射训练**

你选哪一个？
