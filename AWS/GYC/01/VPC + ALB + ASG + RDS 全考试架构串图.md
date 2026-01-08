下面这套是 **SAA-C03 Networking 域“满分模板”**，我按 **考试真实思维路径** 给你：
👉 **先看整体 → 再刷陷阱 → 再抠 NAT → 最后用真题带你选 Subnet / SG / NACL**

---

# 1️⃣ VPC + ALB + ASG + RDS **全考试架构串图（ASCII）**

> **这是 SAA-C03 默认正确架构**（80% 题目都在这张图里变形）

```text
                Internet
                    |
               Route 53 (DNS)
                    |
              Internet Gateway
                    |
            Public Subnets (Multi-AZ)
          --------------------------------
          |                              |
        ALB (HTTPS, TLS)               ALB
          |                              |
   --------------------------------------------
   |                                          |
EC2 / ECS (ASG, Stateless)        EC2 / ECS (ASG)
   |                                          |
   |  (Async / Heavy Tasks?)                  |
   |----------- YES ---------------------------|
                |
              SQS (optional)
                |
         Worker Tier (ASG)
                |
        ---------------------
        |                   |
   Private Subnets (Multi-AZ)
        |
     RDS / Aurora
   (Multi-AZ, no public IP)
```

### 🧠 考试必须脑补的 6 个点

1. **ALB 只在 Public Subnet**
2. **EC2 App 在 Private Subnet**
3. **App 层 Stateless（ASG）**
4. **RDS 几乎永远 Private Subnet**
5. **TLS 在 ALB 终止**
6. **HA = Multi-AZ（不是性能）**

---

# 2️⃣ VPC 陷阱题 **20 道（秒杀误选）**

> 规则：**看到关键词 → 不画图 → 直接选**

---

### Q1

EC2 在 private subnet 访问 RDS（同 VPC）
✅ **不需要 NAT**

---

### Q2

Private subnet EC2 无法 yum install
✅ **NAT Gateway**

---

### Q3

RDS 无法连接，EC2 与 RDS 在同一 subnet
✅ **检查 Security Group**

---

### Q4

Ping 通，但 DB 连接 timeout
✅ **端口 / SG 问题**

---

### Q5

想让 RDS 对公网可访问
✅ **Publicly Accessible = YES + Public Subnet**

---

### Q6

ALB 放在 private subnet
❌ **错**（ALB 必须在 public subnet）

---

### Q7

Multi-AZ RDS failover 后连接失败
✅ **使用 RDS endpoint**

---

### Q8

想限制只有 App 能访问 DB
✅ **SG 引用 SG**

---

### Q9

加了 NACL 后流量异常
✅ **NACL 是 stateless（双向都要放）**

---

### Q10

Subnet 决定安全策略
❌ **错（SG 才是）**

---

### Q11

Private subnet 不能互相通信
❌ **错（默认同 VPC 可达）**

---

### Q12

RDS 需要 NAT 才能被 EC2 访问
❌ **错**

---

### Q13

EC2 有 public IP 就一定在 public subnet
❌ **错（route table 决定）**

---

### Q14

想阻止某个 IP
✅ **NACL（IP 级）**

---

### Q15

想只允许某个服务访问 DB
✅ **Security Group**

---

### Q16

ALB 能直接访问 Internet
❌ **ALB 通过 IGW**

---

### Q17

RDS 安全性最高做法
✅ **Private subnet + SG 限制**

---

### Q18

Private subnet EC2 要访问 S3
✅ **VPC Endpoint（不是 NAT）**

---

### Q19

跨 AZ 高可用
✅ **Multi-AZ**

---

### Q20

VPC 内流量默认被阻断
❌ **错（默认全通）**

---

## 🧠 一句话网络真理

```
能不能连 = Security Group
能不能上网 = Route Table / NAT
Subnet ≠ Firewall
```

---

# 3️⃣ 深挖 **NAT Gateway vs NAT Instance（必考）**

| 对比点  | **NAT Gateway** | **NAT Instance**    |
| ---- | --------------- | ------------------- |
| 托管   | ✅ Fully managed | ❌ 自管 EC2            |
| 高可用  | ✅ 自动            | ❌ 需自己做              |
| 带宽   | ✅ 自动扩展          | ❌ 受实例限制             |
| 运维   | ❌ 无             | ❌ 需打补丁              |
| 成本   | 💲 按流量          | 💲 便宜但风险高           |
| 考试答案 | **默认选它**        | 只有“legacy / cost”才选 |

### 🧠 考试暗号

* **Production / HA / recommended** → NAT Gateway
* **Legacy / cost-sensitive / small workload** → NAT Instance

---

### ❌ 高频误区

* NAT 用来 **访问 RDS** ❌
* NAT 放在 private subnet ❌
* NAT Gateway 需要 SG ❌（它没有）

---

# 4️⃣ 用 **真实考试场景题** 带你选 Subnet / SG / NACL

---

## 🎯 场景 1（最经典）

> Web app must be publicly accessible.
> Database must NOT be accessible from the internet.

**正确选择：**

* ALB → **Public Subnet**
* EC2 App → **Private Subnet**
* RDS → **Private Subnet**
* DB SG → **只允许 App SG**

---

## 🎯 场景 2（安全加强）

> Only allow traffic from specific IP ranges.

**正确选择：**

* **NACL**（IP 级）

---

## 🎯 场景 3（精细控制）

> Only application servers can access database.

**正确选择：**

* **Security Group 引用 Security Group**

---

## 🎯 场景 4（私网出网）

> EC2 in private subnet needs internet access.

**正确选择：**

* **NAT Gateway in public subnet**
* Route table 指向 NAT

---

## 🎯 场景 5（反直觉）

> EC2 can ping DB but cannot connect.

**正确选择：**

* **检查 DB 端口 SG**
* ❌ 不是 subnet / NAT

---

# ⚡ 30 秒 VPC 决策流程（闭眼背）

```text
要不要公网？
  └─ ALB / Bastion → Public Subnet

App / DB？
  └─ Private Subnet

连不上？
  └─ SG → NACL → Endpoint

要上网？
  └─ NAT Gateway

要控 IP？
  └─ NACL
```

---

## 🎯 SAA-C03 生存法则（非常关键）

* **90% 网络错题 = SG 没想清**
* **NAT ≠ 互连**
* **Subnet 决定路由，不决定安全**
* **默认架构永远是：Public ALB + Private App + Private DB**

---

### 下一步（强烈推荐）

* 🔥 **10 道 Networking“反直觉地狱题”**
* 🧠 **SAA-C03 全域 3 秒决策总表（最终版）**
* 📄 **Networking Domain 可下载 PDF**

你直接回我 **A / B / C**，我立刻继续。
