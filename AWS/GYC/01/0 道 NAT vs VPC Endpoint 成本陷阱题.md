好，这一组我直接给你 **Networking 成本&路径“秒杀包”**：
👉 **成本陷阱题 × 决策树 × 一页速记卡 × 专项小卷**（全部按 SAA-C03 考场口径）

---

## 🔥 一、10 道 **NAT vs VPC Endpoint** 成本陷阱题（秒杀型）

> 规则：**看到“成本 / 流量方向 / 访问对象” → 直接选**

---

### Q1

**Private subnet 访问 S3，流量大、成本敏感**
✅ **VPC Gateway Endpoint（S3）**
❌ NAT Gateway（按流量收费）

---

### Q2

**Private EC2 访问 DynamoDB，避免公网**
✅ **VPC Gateway Endpoint（DynamoDB）**
❌ NAT

---

### Q3

**Private EC2 需要访问公网 API（非 AWS）**
✅ **NAT Gateway**
❌ Endpoint（只给 AWS 服务）

---

### Q4

**高吞吐写 S3，NAT 费用暴涨**
✅ **S3 Gateway Endpoint**
❌ NAT（流量计费）

---

### Q5

**访问 CloudWatch Logs / ECR / SSM（AWS 服务）**
✅ **Interface Endpoint**
❌ NAT（可用但更贵）

---

### Q6

**偶发访问 S3，流量很小**
⚠️ **NAT 或 Endpoint 都可**
🔑 但**考试偏向 Endpoint**（最佳实践）

---

### Q7

**需要最小化数据外泄，完全不走公网**
✅ **VPC Endpoint（Gateway / Interface）**
❌ NAT（经公网）

---

### Q8

**Private subnet 访问 S3 + 访问外部 SaaS**
✅ **Endpoint（S3） + NAT**
❌ 只用 NAT（不省钱）

---

### Q9

**跨 AZ 大流量访问 AWS API**
✅ **Endpoint**
❌ NAT（多跳 + 成本）

---

### Q10（反直觉）

**Private EC2 无法访问 S3**
❌ 先加 NAT
✅ **先想 Gateway Endpoint**（更便宜更安全）

---

## 🧠 一句话成本心法

```
访问 AWS 服务 → Endpoint
访问 公网 → NAT
流量大 → Endpoint
省钱 + 私网 → Endpoint
```

---

## 🧠 二、**VPC Endpoint 决策树（Gateway / Interface）**

```text
START
 |
 |-- 访问的是 AWS 服务吗？
 |        |-- NO → NAT Gateway
 |        |-- YES
 |
 |-- 是否是 S3 / DynamoDB？
 |        |-- YES → Gateway Endpoint
 |        |-- NO
 |
 |-- 是否需要私有访问（不走公网）？
 |        |-- YES → Interface Endpoint
 |        |-- NO  → NAT（不推荐）
```

### 🎯 决策口诀（必背）

```
S3 / DynamoDB → Gateway Endpoint
其他 AWS 服务 → Interface Endpoint
非 AWS / 公网 → NAT
```

---

## 🎯 三、**NAT / IGW / Endpoint 一页网络速记卡**

```text
【IGW】
- 公网入口
- Public Subnet
- Inbound + Outbound
- 不计费（本身）

【NAT Gateway】
- Private → Internet（Outbound only）
- 放在 Public Subnet
- 按流量计费 💲
- 访问非 AWS 必须

【VPC Endpoint】
- Private → AWS 服务
- 不走公网
- 省钱 + 更安全
- Gateway：S3 / DynamoDB
- Interface：其他 AWS 服务
```

### 🚫 高频误区

```
❌ NAT 访问 S3 是最佳方案
❌ Endpoint 可以访问公网
❌ IGW 给 Private Subnet 用
```

---

## ⚡ 30 秒网络路径决策（闭眼版）

```text
要不要公网？
  ├─ YES → IGW / NAT
  └─ NO
       |
    访问 AWS？
       ├─ YES → Endpoint
       └─ NO  → NAT
```

---

## 🧪 四、Networking 专项模拟小卷（15 题）

> **A/B/C/D 单选，给答案 + 一句话理由**

---

### Q1

Private subnet 访问 S3，最低成本？
A. NAT
B. IGW
C. **Gateway Endpoint** ✅
D. Interface Endpoint

---

### Q2

Private subnet 访问第三方支付 API？
A. IGW
B. **NAT Gateway** ✅
C. Endpoint
D. Peering

---

### Q3

需要私有访问 ECR 拉镜像
A. NAT
B. IGW
C. **Interface Endpoint** ✅
D. Gateway Endpoint

---

### Q4

Public subnet 的定义是？
A. 有 Public IP
B. **Route table 指向 IGW** ✅
C. 有 NAT
D. 能访问 S3

---

### Q5

NAT Gateway 放在哪？
A. Private subnet
B. **Public subnet** ✅
C. DB subnet
D. 任意

---

### Q6

NACL 的特性是？
A. Stateful
B. **Stateless** ✅
C. 仅 Inbound
D. 仅 Outbound

---

### Q7

SG 的特性是？
A. Stateless
B. 有 Deny
C. **Stateful** ✅
D. 子网级

---

### Q8

Private EC2 无法 yum install
A. IGW
B. **NAT Gateway** ✅
C. Endpoint
D. Route53

---

### Q9

想阻断特定 IP 段
A. SG
B. **NACL** ✅
C. IGW
D. NAT

---

### Q10

RDS 在 private subnet，EC2 连不上
A. NAT
B. **检查 SG** ✅
C. IGW
D. Endpoint

---

### Q11

访问 DynamoDB 私网、免 NAT
A. NAT
B. IGW
C. **Gateway Endpoint** ✅
D. Interface Endpoint

---

### Q12

访问 CloudWatch 私网
A. NAT
B. IGW
C. Gateway Endpoint
D. **Interface Endpoint** ✅

---

### Q13

NAT 能做什么？
A. Inbound
B. **Outbound only** ✅
C. 私网到私网
D. 端口过滤

---

### Q14

Endpoint 的最大优势
A. 高性能
B. **省钱 + 私网安全** ✅
C. 简单
D. 全球加速

---

### Q15（反直觉）

Private EC2 访问 S3 很慢、很贵
A. 加大 NAT
B. **换 Gateway Endpoint** ✅
C. 加 ASG
D. CloudFront

---

## 🎯 考场终极一句话

> **访问 AWS → Endpoint；访问公网 → NAT；能不用 NAT 就不用。**

---

### 下一步我可以继续（你选）：

* 🔥 **20 题 Networking 成本“反直觉地狱卷”**
* 📄 **生成 Networking 成本&路径 PDF（一页）**
* 🧠 **把 NAT / Endpoint / IGW 做成 Anki 闪卡**
* 🎯 **全真 25 题 Networking 压力测试（3 秒/题）**

你回我 **A / B / C / D** 或 **“直接给我 PDF / Anki”**，我立刻交付。
