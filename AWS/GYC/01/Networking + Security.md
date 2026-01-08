太好了，这一轮是 **SAA-C03 最后一层“防失误装甲”**。
我按 **真考反直觉 + 3 秒条件反射 + 可直接导出 PDF** 给你。

---

# 🔥 Part 1：Networking + Security「反直觉挖坑题」20 道

> **规则**：
>
> * 每题只看 **关键词**
> * 问的是 **“为什么你会选错”**

---

### 🕳️ Q1

**关键词**：RDS timeout
✅ **先查 SG / NACL / Route**
❌ 直接升 DB（过度设计）

---

### 🕳️ Q2

**关键词**：Private subnet EC2 无法访问公网
✅ **NAT Gateway + route table**
❌ Internet Gateway（私网走不了）

---

### 🕳️ Q3（高频）

**关键词**：Lambda cannot connect to RDS
✅ **Lambda 没进 VPC**
❌ Lambda timeout 太短

---

### 🕳️ Q4

**关键词**：SG 已放行仍失败
✅ **NACL 是 stateless，漏了 outbound**
❌ SG 配错（烟雾弹）

---

### 🕳️ Q5（反直觉）

**关键词**：RDS not publicly accessible
✅ **这是最佳实践，不是问题**
❌ 打开 Public Access

---

### 🕳️ Q6

**关键词**：HTTPS + global users
✅ **CloudFront**
❌ ALB alone

---

### 🕳️ Q7

**关键词**：TLS certificate management overhead
✅ **ACM**
❌ 自签证书

---

### 🕳️ Q8

**关键词**：WAF + DDoS protection
✅ **CloudFront / ALB + AWS WAF**
❌ EC2 防火墙规则

---

### 🕳️ Q9

**关键词**：Cross-VPC access
✅ **VPC Peering + route table**
❌ Public IP + IGW

---

### 🕳️ Q10

**关键词**：DNS resolves but connection fails
✅ **端口被挡（SG / NACL）**
❌ DNS 配错

---

### 🕳️ Q11

**关键词**：Outbound works, inbound fails
✅ **NACL stateless**
❌ SG stateless（错）

---

### 🕳️ Q12

**关键词**：Secure + least ops
✅ **托管服务（ALB / CloudFront / ACM）**
❌ 自建 Nginx

---

### 🕳️ Q13

**关键词**：HTTPS already on ALB
✅ **CloudFront 仍然有价值**
❌ “已经 HTTPS 就够了”

---

### 🕳️ Q14

**关键词**：Private RDS + EC2 same VPC
✅ **不需要 IGW / NAT**
❌ 网络出口配置（多余）

---

### 🕳️ Q15

**关键词**：Multiple AZ + HA
✅ **ALB / RDS Multi-AZ 是区域级**
❌ 单 AZ 优化

---

### 🕳️ Q16

**关键词**：IAM least privilege
✅ **IAM Role + 精准 Policy**
❌ Access Key 写代码里

---

### 🕳️ Q17

**关键词**：Encryption at rest
✅ **KMS / 默认加密**
❌ 应用层自己加密

---

### 🕳️ Q18

**关键词**：Inbound blocked intermittently
✅ **NACL 顺序规则命中**
❌ 网络抖动

---

### 🕳️ Q19

**关键词**：HTTPS 性能慢
✅ **CloudFront / TLS offload**
❌ 更大实例

---

### 🕳️ Q20（终极）

**关键词**：secure + scalable + minimal ops
✅ **AWS 托管组合拳**
❌ “自己搭一个”

---

📌 **一句话总结**

> **Networking + Security 的坑，90% 是“你想多了”**

---

# 🧠 Part 2：SAA-C03【全域 3 秒决策总表】最终版

> **看到关键词 → 3 秒内选服务**

---

## 🌐 Networking

| 暗号                | 秒选                |
| ----------------- | ----------------- |
| Global users      | CloudFront        |
| EC2 Web           | ALB               |
| Private subnet 出网 | NAT Gateway       |
| 跨 VPC             | VPC Peering       |
| 连不上 RDS           | SG → NACL → Route |

---

## 🔐 Security

| 暗号                | 秒选       |
| ----------------- | -------- |
| HTTPS certificate | ACM      |
| DDoS              | Shield   |
| Web attack        | WAF      |
| least privilege   | IAM Role |
| encryption        | KMS      |

---

## 🧠 Compute / Scaling（联动）

| 暗号            | 秒选                         |
| ------------- | -------------------------- |
| traffic spike | ASG                        |
| task spike    | SQS → Lambda               |
| session lost  | Stateless + external store |

---

## 🧾 Database

| 暗号             | 秒选                   |
| -------------- | -------------------- |
| HA             | Multi-AZ             |
| read heavy     | Read Replica / Cache |
| sub-ms latency | ElastiCache / DAX    |

---

## 📡 Messaging

| 暗号                   | 秒选   |
| -------------------- | ---- |
| fan-out              | SNS  |
| buffer               | SQS  |
| order + exactly once | FIFO |
| retry failure        | DLQ  |

---

## 📌 终极口诀（必须能背）

> “全球先上 CloudFront
> EC2 前放 ALB
> 出网靠 NAT
> 证书交 ACM
> 连不上先查网络
> 能托管就别自建”

---

# 📄 Part 3：可下载 PDF（目录 + 封面内容）

下面内容 **可直接复制导出为 PDF**（Markdown / Notion / Word 都行）。

---

## 📘 封面（PDF）

**Title**

> **AWS SAA-C03
> Networking + Security
> 3-Second Decision Guide**

**Subtitle**

> Final Exam-Ready Cheat Sheet
> （反直觉挖坑专治版）

---

## 📑 目录（PDF）

1. Exam Mindset：为什么“最小解”必赢
2. Networking 反直觉 20 题
3. RDS / VPC 连接失败排查树
4. HTTPS / TLS / ACM 决策速记
5. 全域 3 秒决策总表（最终版）
6. 考前 5 行口诀（最后一页）

---

## 📌 最后一页（建议单独打印）

> “不升规格
> 不自建
> 不绕路
> 不猜 AWS bug
> 先找托管最小解”

---

## 🚀 下一步（最后 48 小时最有效）

1️⃣ **90 分钟全真 SAA-C03 模拟（只剩你最容易错的点）**
2️⃣ **错题本：Networking + Security 专项复盘**
3️⃣ **口述 3 秒反射训练（我出题，你秒答）**

你直接选 **1 / 2 / 3**，我继续陪你冲线。
