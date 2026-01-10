一句话先给你结论：

> **Provisioned infrastructure = 已经被创建、分配、并随时可用的基础设施资源**。

下面我用**对比 + 场景 + AWS 语境**把它彻底讲清楚。

---

## 1️⃣ 从英文直觉理解

### provision = 提前准备 / 配给

### infrastructure = 基础设施（服务器、网络、存储）

👉 **provisioned infrastructure**
= **提前把服务器/资源“配好、开好、放在那里等你用”**

不是临时的、不是用时才现造。

---

## 2️⃣ 和你已经见过的 AWS 东西直接对照

### ✅ 属于 *Provisioned infrastructure* 的

* **EC2 实例**（你 `RunInstances` 之后）
* **EBS 卷**（已经 attach）
* **RDS 实例**
* **ALB / NLB**
* **已创建的 VPC / Subnet / NAT Gateway**
* **Auto Scaling Group 里已启动的实例**

👉 特点：

* 资源 **已经存在**
* **一直占着钱**
* 不管你用不用，它都在那

---

### ❌ 不算 *Provisioned infrastructure* 的（或弱化版）

* **Lambda**
* **S3**
* **DynamoDB On-Demand**
* **Step Functions**

👉 特点：

* 不需要你提前开服务器
* 用的时候才“调度资源”
* 更偏 **serverless**

---

## 3️⃣ 最经典的考试 / 面试对比（你一定会遇到）

### Provisioned infrastructure（EC2 思维）

```text
你先：
- 选实例类型
- 选 AZ
- 启动 EC2
- 配安全组
- 装软件
- 管容量
```

👉 **你负责 capacity + availability**

---

### Serverless / Managed（Lambda 思维）

```text
你只：
- 写代码
- 配权限
- 调 API
```

👉 **AWS 负责 infrastructure**

---

## 4️⃣ 用一句 AWS 官方口径的话翻译

> **Provisioned infrastructure means you must explicitly create and manage compute resources ahead of time.**

中文翻译：

> **你必须提前创建并管理计算资源，而不是按需自动获得。**

---

## 5️⃣ 你刚才那些 IAM / EC2 / Region 限制，为什么老提 provisioned？

因为：

* **EC2 / RDS / ALB** → 都是 provisioned
* AWS 必须防止你：

  * 起太大的机器
  * 起太多机器
  * 起在错误的 region
  * 一直占着钱

所以 policy 里会看到：

```json
ec2:RunInstances
Deny if InstanceType != t3.micro
Deny if VolumeSize > 8
Deny if Region != us-east-1
```

👉 **这是典型的“控制 provisioned infrastructure 成本和风险”**

---

## 6️⃣ 一个非常面试友好的总结句（建议背）

> **Provisioned infrastructure refers to compute or storage resources that are explicitly created, allocated, and continuously available, requiring capacity planning and lifecycle management.**

更口语一点：

> **It means servers are already up and running before traffic comes in, and you pay for them whether you use them or not.**

---

## 7️⃣ 最后用一张极简对照表收尾

| 维度     | Provisioned Infrastructure | Serverless  |
| ------ | -------------------------- | ----------- |
| 是否提前创建 | ✅ 是                        | ❌ 否         |
| 是否一直存在 | ✅ 是                        | ❌ 否         |
| 是否一直收费 | ✅ 是                        | 按调用         |
| 例子     | EC2 / RDS                  | Lambda / S3 |
| 运维负担   | 高                          | 低           |

---

如果你愿意，下一步我可以帮你把
**“provisioned / managed / serverless”**
整理成一张 **AWS 考试暗号反射表**，看到关键词直接秒选。
