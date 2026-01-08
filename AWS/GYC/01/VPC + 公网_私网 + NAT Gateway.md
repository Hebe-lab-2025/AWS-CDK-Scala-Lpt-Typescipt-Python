非常好，这一组是 **SAA-C03 网络 + 数据库排错的“拿分区”**。
我按你给的顺序：**网络图 → 真题 → 速记卡 → 陷阱训练**，一次性给你打穿。

---

## 🔹 一、VPC + 公网 / 私网 + NAT Gateway（详细 ASCII 网络图）

```
                    Internet
                        |
                 ┌────────────┐
                 │ Internet GW│
                 └────────────┘
                        |
        ┌─────────────────────────────────┐
        │               VPC               │
        │                                 │
        │  Public Subnet (AZ-a)           │
        │  ┌───────────────┐              │
        │  │  ALB / EC2    │  ← 有公网 IP  │
        │  └───────────────┘              │
        │          |                      │
        │      ┌────────┐                │
        │      │  NAT   │  ← 只出不进     │
        │      │Gateway │                │
        │      └────────┘                │
        │          |                      │
        │  Private Subnet (AZ-a)          │
        │  ┌───────────────┐              │
        │  │  App EC2/ECS  │  ← 无公网 IP │
        │  └───────────────┘              │
        │          |                      │
        │  ┌───────────────┐              │
        │  │   RDS DB      │  ← 私网 only │
        │  └───────────────┘              │
        │                                 │
        └─────────────────────────────────┘
```

### 🧠 考试必背 3 句话

* **Internet Gateway**：让 VPC 能“被访问”
* **NAT Gateway**：让私网“访问外网”
* **RDS**：❌ 永远不需要公网访问（考试默认）

---

## 🔹 二、数据库无法连接（AWS 真题风格练习）

### 🧪 题目 1（超高频）

> An application running on EC2 cannot connect to an RDS database in the same VPC.
> The database is healthy. What is the MOST likely cause?

**正确答案：**
👉 **Security Group inbound rule is missing**

❌ 常见错误选项

* RDS needs a public IP
* Need NAT Gateway
* Need Internet Gateway

---

### 🧪 题目 2

> EC2 in a public subnet cannot connect to RDS in a private subnet.

👉 **这是正常的**
只要 **Security Group 允许**，就一定能连。

---

### 🧪 题目 3

> EC2 in a private subnet cannot connect to RDS, and cannot access the internet.

👉 **这是两个问题**

* DB 连接问题 → **Security Group**
* 上网问题 → **NAT Gateway / Route Table**

---

### 🧪 题目 4

> After enabling Multi-AZ, the application cannot connect to the database endpoint.

👉 **应用写死了 IP，而不是 RDS Endpoint**
（真·陷阱）

---

## 🔹 三、RDS 连接问题排查速记卡（考场 10 秒用）

### 🧠 RDS 连不上 = 永远按这个顺序查

```
① Security Group（90% 的问题）
   |
② Subnet（是否在 VPC 内）
   |
③ Route Table（同 VPC 通不通）
   |
④ Endpoint（有没有用 RDS endpoint）
   |
⑤ NACL（极少考）
```

---

### 📌 Security Group 检查清单（必背）

* RDS inbound 是否允许：

  * 来源：**EC2 的 SG**
  * 端口：3306 / 5432
* ❌ 不是 0.0.0.0/0
* ❌ 不是公网 IP

---

## 🔹 四、“考试陷阱题”：一眼判断问题在哪一层

### 🔥 陷阱 1

> EC2 can access the internet but cannot connect to RDS.

👉 **数据库层问题**

* Security Group（不是 NAT）

---

### 🔥 陷阱 2

> EC2 cannot connect to RDS and cannot download patches.

👉 **两层问题**

* RDS：Security Group
* Internet：NAT Gateway / Route Table

---

### 🔥 陷阱 3

> RDS is in a private subnet and users cannot access it directly from the internet.

👉 **这是正确设计**
❌ 不需要改

---

### 🔥 陷阱 4

> The solution requires secure database access without exposing it publicly.

👉 **Private Subnet + Security Group**
❌ 公网 IP 是错误答案

---

## 🧠 终极“网络 + DB”定位口诀（闭眼用）

```
连不上 DB？
   |
  先看 SG（一定考）
   |
再看是否同 VPC
   |
再看 Endpoint
   |
最后才是路由 / NACL
```

---

## ✅ 考前 10 秒自检

* ❌ RDS 不靠 NAT 连接 EC2
* ❌ RDS 不需要公网 IP
* ✅ 99% DB 问题 = **Security Group**
* ✅ NAT 只解决 **出网**，不解决 **内网访问**

---

如果你愿意，下一步我可以继续给你：

1️⃣ **10 道 VPC + RDS 网络混合陷阱题（秒杀型）**
2️⃣ **SAA-C03 网络 & 数据库联合决策树（终极版）**
3️⃣ **把所有 VPC / NAT / SG / RDS 做成一页 PDF Cheat Sheet**

你选一个，我立刻继续。
