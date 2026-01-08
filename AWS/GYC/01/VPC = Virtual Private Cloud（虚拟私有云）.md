## VPC 是啥？（一句话先记）

> **VPC = Virtual Private Cloud（虚拟私有云）**
> 👉 **在 AWS 里给你单独划一块“私有网络”，你完全可控**

---

## 一、用现实世界的比喻理解 VPC（最好记）

> **VPC 就像你在 AWS 里的“私人小区”**

| 现实世界 | AWS                   |
| ---- | --------------------- |
| 小区   | **VPC**               |
| 楼栋   | Subnet                |
| 门禁   | Security Group / NACL |
| 门牌号  | IP                    |
| 出入口  | Internet Gateway      |

---

## 二、VPC 解决什么问题？

在 **没有 VPC** 的情况下：

* 所有资源在公共网络
* 不安全
* 无法隔离

有了 **VPC**：

* 🔒 网络隔离
* 🎯 自定义 IP 范围
* 🧱 控制进出流量
* 🏗️ 搭私有架构

---

## 三、VPC 里最重要的组成部分（考试必考）

![Image](https://docs.aws.amazon.com/images/prescriptive-guidance/latest/integrate-third-party-services/images/p2_vpc-peering.png)

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/vpc-example-private-subnets.png)

### 1️⃣ **CIDR（IP 地址范围）**

* 例如：`10.0.0.0/16`
* 决定你这个 VPC 能用多少 IP

---

### 2️⃣ **Subnet（子网）**

* VPC 的一部分
* **必须属于一个 AZ**

| 类型             | 能不能上网 |
| -------------- | ----- |
| Public Subnet  | ✅     |
| Private Subnet | ❌（默认） |

---

### 3️⃣ **Internet Gateway（IGW）**

* VPC 访问公网的“门”

📌 **没有 IGW = 上不了网**

---

### 4️⃣ **Route Table（路由表）**

* 决定流量“往哪走”

---

### 5️⃣ **Security Group（安全组）**

* 实例级防火墙
* **有状态（stateful）**

---

### 6️⃣ **NACL（网络 ACL）**

* 子网级防火墙
* **无状态（stateless）**

---

## 四、一个最经典的考试架构（脑中要有）

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/vpc-example-private-subnets.png)

![Image](https://miro.medium.com/1%2AI_QVgZ-HpE_q-zDFGC4Kmg.png)

```
VPC (10.0.0.0/16)
 ├─ Public Subnet
 │   └─ ALB + IGW
 └─ Private Subnet
     ├─ EC2 / ECS
     └─ RDS
```

📌 **数据库一定在 Private Subnet（考试铁律）**

---

## 五、VPC 和 EC2 的关系（常考）

> ❓ EC2 能脱离 VPC 吗？
> ❌ 不能

👉 **所有 EC2 都必须在某个 VPC 里**

---

## 六、考试里怎么“秒识别 VPC 题”

看到这些关键词，**就想到 VPC**：

* private network
* network isolation
* IP address range
* public / private subnet
* security group

---

## 七、30 秒口试 / 考试标准回答

> **A VPC is a logically isolated virtual network in AWS that allows you to define IP ranges, subnets, routing, and security controls for your resources.**

---

## 八、一句话中文速记版

> **VPC = AWS 里的“私有网络地盘”**

---

如果你愿意，我可以下一步帮你：

* 🔹 **画一张 VPC 全组件 ASCII 架构图**
* 🔹 **讲清楚 Public Subnet vs Private Subnet**
* 🔹 **出 10 道 VPC 高频考试题**
* 🔹 **对比 Security Group vs NACL（秒选题）**

你想继续 **网络** 还是 **刷题**？
