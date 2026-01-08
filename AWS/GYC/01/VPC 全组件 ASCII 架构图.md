下面这套是 **SAA-C03 必背版 VPC 模块**，按你给的 4 点一次性讲透。

---

## 一、VPC 全组件 **ASCII 架构图（考试标准）**

```
                    Internet
                        |
                   +----IGW----+
                   |           |
        Public Subnet A     Public Subnet B
        (AZ-a)              (AZ-b)
      +-------------+     +-------------+
      |   ALB /     |     |   Bastion   |
      |   NAT GW    |     |   Host      |
      +------+------+     +------+------+
             |                     |
             |                     |
      ---------------- VPC ----------------
             |                     |
      Private Subnet A      Private Subnet B
        (AZ-a)                (AZ-b)
      +-------------+       +-------------+
      |  EC2 / ECS  |       |  EC2 / ECS  |
      |  App Tier   |       |  App Tier   |
      +------+------+       +------+------+
             |                     |
             |                     |
        +-------------------------------+
        |        RDS / DynamoDB         |
        |        (Data Tier)            |
        +-------------------------------+

关键组件：
- VPC
- Subnets (Public / Private)
- Internet Gateway (IGW)
- NAT Gateway
- Route Table
- Security Group
- NACL
```

📌 **一句话定位**

> **VPC = AWS 上的私有网络边界**

---

## 二、Public Subnet vs Private Subnet（必考本质）

### 🔓 Public Subnet

**定义（考试用）**

> A subnet whose route table has a route to an Internet Gateway.

**特点**

* 关联 **IGW**
* 可以有 **Public IP**
* 能 **直接访问 Internet**

**典型资源**

* ALB
* Bastion Host
* NAT Gateway

---

### 🔒 Private Subnet

**定义（考试用）**

> A subnet without a direct route to the Internet Gateway.

**特点**

* **没有 IGW 路由**
* 默认 **无法直接上网**
* 出网需要 **NAT Gateway**

**典型资源**

* EC2 App
* RDS
* 内部服务

---

### 🧠 秒选对比表

| 维度      | Public Subnet | Private Subnet |
| ------- | ------------- | -------------- |
| IGW 路由  | ✅ 有           | ❌ 无            |
| 是否能直连公网 | ✅             | ❌              |
| 是否更安全   | ❌             | ✅              |
| 常见资源    | ALB / NAT     | EC2 / RDS      |

📌 **考试陷阱**
❌ 有 Public IP ≠ Public Subnet
✔️ **Route Table 才是决定因素**

---

## 三、10 道 Amazon VPC 高频考试题（秒选）

### Q1

**VPC 的核心作用是？**
✅ 提供隔离的私有网络

---

### Q2

**什么决定一个 Subnet 是 Public 还是 Private？**
✅ Route Table 是否指向 IGW

---

### Q3

**Private Subnet 的实例如何访问 Internet？**
✅ NAT Gateway

---

### Q4

**IGW 的作用是？**
✅ 让 VPC 与 Internet 通信

---

### Q5

**NAT Gateway 应部署在哪？**
✅ Public Subnet

---

### Q6

**RDS 推荐放在哪？**
✅ Private Subnet

---

### Q7

**跨 AZ 高可用，至少需要什么？**
✅ Multiple Subnets in different AZs

---

### Q8

**Security Group 属于哪一层？**
✅ Instance level

---

### Q9

**NACL 的规则特点？**
✅ Stateless（无状态）

---

### Q10

**默认 VPC 是否有 IGW？**
✅ 有

---

## 四、Security Group vs NACL（**秒选题王炸**）

### 🔐 Security Group（SG）

| 特点   | 说明           |
| ---- | ------------ |
| 作用层级 | **实例级**      |
| 状态   | **Stateful** |
| 规则   | 只允许（Allow）   |
| 默认   | 拒绝所有入站       |
| 常用   | 精细控制         |

📌 记忆句

> **Security Group remembers traffic**

---

### 🚧 Network ACL（NACL）

| 特点   | 说明            |
| ---- | ------------- |
| 作用层级 | **Subnet 级**  |
| 状态   | **Stateless** |
| 规则   | Allow + Deny  |
| 顺序   | 规则号决定         |
| 常用   | 粗粒度隔离         |

📌 记忆句

> **NACL forgets traffic**

---

### ⚔️ 终极对比表（考试直接选）

| 维度   | Security Group | NACL      |
| ---- | -------------- | --------- |
| 级别   | Instance       | Subnet    |
| 状态   | Stateful       | Stateless |
| Deny | ❌              | ✅         |
| 顺序   | ❌              | ✅         |
| 默认   | 全拒入站           | 全允许       |

---

## 五、考试终极一句话（必背）

> **A public subnet is determined by its route to an Internet Gateway, while Security Groups provide stateful instance-level protection and NACLs provide stateless subnet-level control.**

---

### 你下一步可以选（直接回数字）👇

1️⃣ 画 **VPC + ALB + ASG + RDS** 全考试架构串图
2️⃣ 出 **VPC 陷阱题 20 道（秒杀误选）**
3️⃣ 深挖 **NAT Gateway vs NAT Instance**
4️⃣ 用 **真实考试场景题** 带你选 Subnet / SG / NACL

你选哪一个？
