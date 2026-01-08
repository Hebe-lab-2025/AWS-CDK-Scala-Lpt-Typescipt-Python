太好了，这一组我直接按 **「考试实战训练包」** 给你做，全是 **SAA-C03 / 面试** 真正会用到的东西。

---

## 一、VPC + 公网 / 私网 + NAT **详细网络 ASCII 图（考试标准）**

```
                           Internet
                               |
                          +---- IGW ----+
                          |             |
                 ┌────────▼────────┐
                 │   Public Subnet  │   (AZ-a)
                 │  Route: 0.0.0.0  │
                 │        → IGW     │
                 │                 │
                 │  +------------+ │
                 │  |  ALB       | │  ← 接收公网流量
                 │  +------------+ │
                 │                 │
                 │  +------------+ │
                 │  | NAT Gateway| │  ← 出口
                 │  | (Public IP)| │
                 │  +------------+ │
                 └────────┬────────┘
                          |
        ------------------------------------------------
                          |
                 ┌────────▼────────┐
                 │  Private Subnet │   (AZ-a)
                 │ Route: 0.0.0.0  │
                 │        → NAT    │
                 │                 │
                 │  +------------+ │
                 │  | EC2 / ECS  | │  ← App Server
                 │  +------------+ │
                 │                 │
                 │  +------------+ │
                 │  | RDS        | │  ← Database
                 │  +------------+ │
                 └─────────────────┘
```

### 🔑 考试必须会的 4 个方向

* **公网 → ALB → 私网 EC2**（入站）
* **私网 EC2 → NAT → Internet**（出站）
* **RDS 永远不直连公网**
* **NAT Gateway 必须在 Public Subnet**

---

## 二、AWS 架构考题练习（真实考试风格）

### 🧪 题 1（秒选型）

**Private Subnet 中的 EC2 需要访问外部 API，但不能被公网访问，应使用什么？**

A. Internet Gateway
B. Elastic IP
C. **NAT Gateway** ✅
D. VPC Peering

📌 **关键词**：outbound only / private subnet

---

### 🧪 题 2（陷阱题）

**一个 EC2 有 Public IP，但仍然无法访问 Internet，最可能原因是？**

A. Security Group 拒绝
B. **Subnet Route Table 没有指向 IGW** ✅
C. 没有 NAT
D. NACL 是 Stateless

📌 **结论**：Subnet 是否 public **只看 Route Table**

---

### 🧪 题 3（架构设计）

**以下哪个组件不应该放在 Public Subnet？**

A. ALB
B. Bastion Host
C. NAT Gateway
D. **RDS** ✅

---

### 🧪 题 4（高频）

**为了实现高可用 NAT，正确做法是？**

A. 一个 NAT Gateway 跨 AZ 使用
B. **每个 AZ 一个 NAT Gateway** ✅
C. 用 Security Group
D. 用 Internet Gateway

---

## 三、数据库无法连接排查 Checklist（🔥 超实用）

> **场景**：App → RDS 连接失败
> **考试 & 实战通用**

---

### ✅ Step 1：网络层（80% 问题在这）

* [ ] **RDS 是否在 Private Subnet**
* [ ] App 和 RDS 是否在 **同一个 VPC**
* [ ] Route Table 是否正确（无需 IGW）
* [ ] 是否误放在不同 AZ / VPC

---

### ✅ Step 2：Security Group（最常见）

* [ ] RDS SG 是否 **允许 App SG**
* [ ] 端口是否正确（3306 / 5432）
* [ ] **不能用 IP，必须用 SG 引用**

✅ 正确写法

```
Inbound:
MySQL 3306
Source: App-SG
```

---

### ✅ Step 3：NACL（容易被忽略）

* [ ] 入站端口允许？
* [ ] 出站端口允许？
* [ ] Ephemeral Ports (1024–65535) 是否放行？

📌 **NACL 是 Stateless**

---

### ✅ Step 4：DNS / Endpoint

* [ ] 用的是 RDS Endpoint，不是 IP
* [ ] 没把 RDS Endpoint 写成 localhost
* [ ] VPC DNS Hostname 已启用

---

### ✅ Step 5：应用层（最后才看）

* [ ] 用户名 / 密码
* [ ] SSL 参数（requireSSL）
* [ ] 连接池是否耗尽

---

### 🎯 一句话总结

> **连不上 RDS，先查 SG，再查 Subnet，再查 NACL，最后才是代码**

---

## 四、真实考试题目风格训练（综合题）

### 🧠 场景题（必考）

> A company deploys EC2 instances in private subnets.
> The instances must download software updates from the Internet,
> but must not be accessible from the Internet.

**最优架构是？**

A. Public Subnet + IGW
B. Private Subnet + IGW
C. **Private Subnet + NAT Gateway in Public Subnet** ✅
D. Private Subnet + Elastic IP

📌 **标准答案句**

> Use a NAT Gateway in a public subnet to allow outbound Internet access from private subnets.

---

## 五、终极考试口诀（背这个）

```
Public Subnet = Route to IGW
Private Subnet = No IGW
NAT = Outbound only
RDS = Always private
SG = Stateful
NACL = Stateless
```

---

### 下一步我可以继续帮你（直接回数字）👇

1️⃣ 画 **VPC + ALB + ASG + RDS** 完整三层考试图
2️⃣ 出 **VPC / NAT / SG / NACL 陷阱题 20 道**
3️⃣ 专练 **“为什么连不上 DB” 场景题 10 道**
4️⃣ 做一套 **SAA-C03 VPC 模拟小卷（15 题）**

你选哪个？
