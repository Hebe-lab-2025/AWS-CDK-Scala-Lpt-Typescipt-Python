下面给你一套**面试 + 白板 + 真实事故**都能直接用的 AWS 网络速记包。
（结构 = 决策树 → 追问 → 存储三选一 → 真实事故复盘）

---

## 🧠 Public vs Private Subnet「**决策树**」（30 秒白板版）

![Image](https://www.testpreptraining.com/blog/wp-content/uploads/2022/04/What-is-the-difference-between-Public-and-Private-Subnet-in-AWS-2.png)

![Image](https://miro.medium.com/1%2Agftv4LSqU_12kRqNwYISJw.png)

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/vpc-example-private-subnets.png)

![Image](https://docs.aws.amazon.com/images/prescriptive-guidance/latest/load-balancer-stickiness/images/roundtrip.png)

```text
❓ 这个资源需要被「公网直接访问」吗？
│
├─ YES
│   ├─ 是否必须直接暴露 IP？
│   │   ├─ YES → Public Subnet（极少数）
│   │   └─ NO  → ALB in Public + Backend in Private ✅
│
└─ NO
    ├─ 是否需要访问互联网（拉包 / 调 API）？
    │   ├─ YES → Private Subnet + NAT Gateway
    │   └─ NO  → Private Subnet（最安全）
```

### 一句话总结（面试官最爱）

> **“能不在 Public，就一定放 Private；对外访问用 ALB，不用 EC2 直连公网。”**

---

## 🧪 10 道「**为什么一定要放 Private Subnet**」追问（90% 会卡）

### 1️⃣ 为什么数据库一定要在 Private？

**答**：

* DB 不需要公网入口
* Public = 增加攻击面
* 正确路径：`App (Private) → DB (Private)`

---

### 2️⃣ ALB 为什么在 Public，而 EC2 在 Private？

**答**：

* ALB = 入口
* EC2 = 业务逻辑
* **分层隔离 = 安全 + 可控**

---

### 3️⃣ Private Subnet 里 EC2 怎么访问互联网？

**答**：
→ **NAT Gateway（在 Public Subnet）**

```text
EC2 (Private)
 → Route Table: 0.0.0.0/0 → NAT
 → NAT → IGW → Internet
```

---

### 4️⃣ 直接给 EC2 公网 IP 不行吗？

**答（关键点）**：

* 能，但 **不该**
* 不可审计、不易封锁
* 违反 **least privilege**

---

### 5️⃣ 没有 NAT 会发生什么？

**答**：

* `yum install` / `apt update` 失败
* SDK 拉不到依赖
* ECS Task 启不来（最常见）

---

### 6️⃣ Lambda 在 VPC 里为什么“突然没网”？

**答**：

* Lambda ENI 在 **Private Subnet**
* **没 NAT = 无法出网**

---

### 7️⃣ Bastion Host 为什么是 Public？

**答**：

* 只负责 **跳板**
* 内网资源仍然 Private
* SSH 不直接打业务机

---

### 8️⃣ Security Group 够了，还要 Private Subnet？

**答**：

* SG 是**逻辑防火墙**
* Subnet 是**网络层隔离**
* 两者是 **叠加，不是替代**

---

### 9️⃣ Private Subnet 能不能被访问？

**答**：

* **能**（ALB / VPC Peering / VPN）
* 但**不能被公网直接访问**

---

### 🔟 面试官终极追问

> “如果你把 Backend 放 Public，会发生什么？”

**标准答法**：

* 攻击面扩大
* 容易被扫描
* 违反 AWS Well-Architected Security Pillar

---

## 🔁 EFS vs EBS vs S3「**血虐三选一**」

![Image](https://k21academy.com/wp-content/uploads/2020/12/s3-vs-ebs-vs-efs-dzone.png)

![Image](https://docs.aws.amazon.com/images/efs/latest/ug/images/efs-ec2-how-it-works-Regional_china-world.png)

![Image](https://docs.aws.amazon.com/images/ebs/latest/userguide/images/volume-lifecycle.png)

![Image](https://cdn.prod.website-files.com/63403546259748be2de2e194/6510ab90d75705217fe92a97_Img1b_Short2.gif)

### 🧪 面试快选表

| 场景            | 正确答案    | 原因           |
| ------------- | ------- | ------------ |
| 单 EC2 系统盘     | **EBS** | Block + 高性能  |
| 多 EC2 共享文件    | **EFS** | 多挂载          |
| 静态文件 / 图片     | **S3**  | 对象存储         |
| ECS 多 Task 共享 | **EFS** | 横向扩展         |
| 日志长期保存        | **S3**  | 便宜 + Durable |
| 数据库存储         | **EBS** | 低延迟          |

---

### 🚨 经典误区（真考）

* ❌ 用 EBS 给多个 EC2 共享
* ❌ 用 S3 当文件系统（不能 mount）
* ❌ 用 EFS 当数据库磁盘（延迟高）

---

### 一句话决策公式

```text
Block + 单机 → EBS
File + 多机 → EFS
Object + 海量 → S3
```

---

## 🚨 真实事故复盘：Subnet / NAT / Route Table 配错 → **502**

![Image](https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fn0wwoxwbf503fq8ictu8.webp)

![Image](https://miro.medium.com/1%2A1uaR1Pt6pIQ4kIFeNS_KDQ.png)

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/internet-gateway-basics.png)

### 🧯 事故现象

* ALB 返回 **502 Bad Gateway**
* Target 健康检查失败
* ECS Task 一直重启

---

### 🔍 根因链路（面试必会）

```text
ECS Task (Private)
 → 需要拉镜像 / 访问外部 API
 → Route Table 没有 0.0.0.0/0 → NAT
 → 出不了网
 → Task 启动失败
 → ALB 502
```

---

### ✅ 正确修复

1. NAT Gateway 放 **Public Subnet**
2. Private Route Table：

   ```text
   0.0.0.0/0 → NAT Gateway
   ```
3. ALB 只负责入口，不负责出网

---

### 🧠 面试复盘金句

> **“502 不是 ALB 的问题，而是后端没活着。”**

---

## 🎯 面试 30 秒总结模板（直接背）

> “Public Subnet 只放入口组件（ALB / NAT / Bastion），
> 业务和数据库全部放 Private Subnet，
> 出网统一走 NAT，
> 存储按 Block / File / Object 选 EBS / EFS / S3，
> 这是 AWS 安全和可扩展的标准做法。”

---

如果你愿意，我可以下一步帮你：

* 🧠 **画一张 VPC + ALB + ECS + NAT 完整白板图**
* 🧪 **出 5 道 Subnet / Route Table / NAT 混合陷阱题**
* 🔄 **把这套话术改成 Amazon / Google / Startup 三种面试版本**
