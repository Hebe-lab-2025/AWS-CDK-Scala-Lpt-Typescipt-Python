下面给你一整套 **SAA-C03 Compute 专项**（含：15 道陷阱题 + 一页脑图 + 决策树 + 关键词秒选表）。可直接复制到 GitHub/Obsidian。

---

## 1️⃣ Compute vs Database 高频陷阱题 15 道（真题风格）

> 规则：每题只有 1 个最佳答案。每题后面给 **秒杀点**（为什么）+ **其它选项错在哪**。

### Q1

公司要跑一个 **短时、突发** 的图片缩放任务，**事件触发**，不想管服务器。
A. EC2 Auto Scaling
B. Lambda
C. ECS on EC2
D. RDS Multi-AZ
✅ **B**

* 秒杀点：事件触发 + 不管服务器 = **Lambda**
* 错因：A/C 仍要管容量/容器；D 是数据库 HA，不是计算

---

### Q2

一个 Web 应用突然流量暴涨，网站变慢。你发现瓶颈是 **数据库读压力**。最佳做法？
A. 把 EC2 升级更大
B. RDS Multi-AZ
C. RDS Read Replica
D. 把数据库搬到 EC2 自建
✅ **C**

* 秒杀点：**读压力 = Read Replica**（扩读性能）
* 错因：B 是 HA，不提升读吞吐；A 只救 Web 层；D 运维地狱

---

### Q3

订单系统需要 **强一致写入**，且数据库要容灾，AZ 故障可自动切换。
A. RDS Read Replica
B. RDS Multi-AZ
C. ElastiCache Redis
D. DynamoDB DAX
✅ **B**

* 秒杀点：**自动 failover + 同步 standby = Multi-AZ**
* 错因：A 多为异步复制；C/D 是缓存/加速不是主库容灾

---

### Q4

一个数据处理作业需要运行 **2 小时**，每天固定一次，代码可打包成容器，想要**最少运维**。
A. Lambda
B. Fargate
C. EC2 Spot 单机
D. Aurora Serverless
✅ **B**

* 秒杀点：长任务 + 容器 + 不想管服务器 = **Fargate**
* 错因：A 有时长限制；C 省钱但运维多；D 是数据库

---

### Q5

你要部署一个需要 **GPU** 的推理服务，想要弹性扩缩。
A. Lambda
B. ECS/Fargate
C. EC2 (GPU instance) + Auto Scaling
D. RDS Proxy
✅ **C**

* 秒杀点：GPU 主要还是 **EC2 GPU 实例**（ASG 扩缩）
* 错因：Fargate 对 GPU 支持受限/场景受限；D 是数据库连接池

---

### Q6

团队要把一个单体应用快速迁到 AWS，**不改代码**，希望平台托管、自动扩缩、负载均衡。
A. Elastic Beanstalk
B. ECS
C. Lambda
D. DynamoDB
✅ **A**

* 秒杀点：**不改代码 + 托管 Web 平台 = Beanstalk**
* 错因：ECS/Lambda 需要更多改造；D 不是计算

---

### Q7

应用要跑在容器里，但你希望 **自己控制底层 EC2**（例如自定义 AMI、守护进程）。
A. ECS on EC2
B. Fargate
C. Lambda
D. SQS
✅ **A**

* 秒杀点：要控制主机 = **ECS on EC2**
* 错因：B 不给你主机；C 无主机；D 是队列

---

### Q8

你要让 Web 层和后台处理解耦，避免高峰期 Web 卡死。
A. SNS
B. SQS
C. RDS Multi-AZ
D. ElastiCache
✅ **B**

* 秒杀点：**解耦 + 缓冲 + worker 拉取 = SQS**
* 错因：SNS 是推送广播；C/D 不解决解耦

---

### Q9

系统需要把“同一事件”同时发给 **多个订阅者服务**（邮件、审计、风控）。
A. SQS
B. SNS
C. Lambda@Edge
D. Aurora
✅ **B**

* 秒杀点：一对多广播 = **SNS**
* 错因：SQS 一般一条消息被一个 consumer 处理

---

### Q10

数据库连接数被打爆，你要在不改太多代码的情况下缓解连接风暴。
A. ElastiCache
B. RDS Proxy
C. DynamoDB
D. EC2 扩容
✅ **B**

* 秒杀点：连接池/复用 = **RDS Proxy**
* 错因：A 可能减读但不是连接池；D 治标不治本

---

### Q11

你需要 **亚毫秒级**读取用户会话/热点数据，允许最终一致，成本要低。
A. RDS Multi-AZ
B. ElastiCache Redis
C. S3
D. EBS
✅ **B**

* 秒杀点：亚毫秒 + 热点数据 = **Redis/Memcached**
* 错因：A 是数据库；S3/EBS 不是低延迟 KV

---

### Q12

想在全球边缘就近执行轻量逻辑（重定向、鉴权、A/B），降低延迟。
A. Lambda
B. Lambda@Edge / CloudFront Functions
C. ECS
D. RDS Read Replica
✅ **B**

* 秒杀点：边缘执行 = **@Edge / Functions**
* 错因：A 在区域；C 不在边缘；D 是数据库

---

### Q13

一个服务要做到 **请求量不稳定**，并且希望按请求付费，几乎不需要预估容量。
A. EC2 Reserved Instances
B. Lambda
C. EC2 On-Demand
D. RDS Provisioned IOPS
✅ **B**

* 秒杀点：不稳定 + 按调用计费 = **Lambda**
* 错因：EC2 都要容量规划；D 是存储

---

### Q14

批处理任务可中断，想要最低成本跑大规模计算。
A. EC2 On-Demand
B. EC2 Spot
C. Lambda
D. Aurora Serverless
✅ **B**

* 秒杀点：可中断 + 最便宜算力 = **Spot**
* 错因：C 受限；D 是数据库

---

### Q15

你想让应用在同一 VPC 内做服务发现和流量治理（路由、熔断、可观测）。
A. API Gateway
B. App Mesh
C. CloudFront
D. S3
✅ **B**

* 秒杀点：服务网格治理 = **App Mesh**
* 错因：A 是入口 API；C 是 CDN；D 是存储

---

## 2️⃣ 一页考试脑图：所有 Compute 服务（Markdown）

```text
Compute (SAA-C03)
├─ EC2 (VM)
│  ├─ On-Demand (灵活)
│  ├─ Reserved / Savings Plans (省钱长期)
│  ├─ Spot (可中断最便宜)
│  ├─ Dedicated Host/Instance (合规/隔离)
│  ├─ Auto Scaling (弹性扩缩)
│  └─ Placement Group (低延迟/高吞吐/容灾策略)
│
├─ Containers
│  ├─ ECS (AWS 原生容器编排)
│  │  ├─ ECS on EC2 (你管主机)
│  │  └─ Fargate (不管主机，按资源计费)
│  ├─ EKS (Kubernetes)
│  └─ ECR (镜像仓库)
│
├─ Serverless
│  ├─ Lambda (事件驱动、按调用计费)
│  ├─ Lambda@Edge / CloudFront Functions (边缘执行)
│  └─ Step Functions (编排/状态机)
│
├─ PaaS / App Hosting
│  ├─ Elastic Beanstalk (快速上云，不改代码)
│  ├─ App Runner (容器/代码直接托管 Web)
│  └─ Lightsail (轻量一体化)
│
└─ Batch / Big Compute
   ├─ AWS Batch (批处理编排，底层多用 EC2/Spot)
   └─ EMR (大数据/Spark，偏数据计算域)
```

---

## 3️⃣ SAA-C03 真题风格 Compute 决策树（秒选版）

```text
Start: 题目要“跑代码/算力”吗？
├─ 是“事件触发 + 短任务 + 不想管服务器”？
│    └─ Lambda
│
├─ 是“要跑在边缘（全球低延迟）”？
│    └─ CloudFront Functions / Lambda@Edge
│
├─ 是“容器应用”？
│    ├─ 想不管主机？
│    │    └─ Fargate
│    └─ 想控制主机/自定义 AMI/守护进程？
│         └─ ECS on EC2 (或 EKS on EC2)
│
├─ 是“传统应用/需要完整 OS/安装软件/长时间运行”？
│    ├─ 流量不稳定要自动扩缩？
│    │    └─ EC2 + Auto Scaling + ALB
│    ├─ 可中断且想最便宜？
│    │    └─ EC2 Spot (可配 ASG/Batch)
│    └─ 长期稳定要省钱？
│         └─ Reserved Instances / Savings Plans
│
├─ 是“批处理/队列堆积，按作业规模扩缩”？
│    └─ AWS Batch (底层常用 Spot)
│
└─ 是“想最快上云，不太改代码，平台托管”？
     └─ Elastic Beanstalk / App Runner
```

---

## 4️⃣ “看到关键词 → 秒选服务”对照表（高频考点）

| 关键词/暗号                                | 秒选服务                                   | 一句话理由                 |
| ------------------------------------- | -------------------------------------- | --------------------- |
| event-driven / triggers / no servers  | **Lambda**                             | 事件触发、免运维              |
| edge / global low latency logic       | **CloudFront Functions / Lambda@Edge** | 边缘就近执行                |
| containers + no host management       | **Fargate**                            | 不管 EC2                |
| containers + need host control        | **ECS on EC2 / EKS on EC2**            | 你管主机                  |
| lift-and-shift / minimal code change  | **Elastic Beanstalk**                  | PaaS 快速托管             |
| unpredictable traffic + scale out     | **ASG + ALB + EC2**                    | 弹性扩缩标准解               |
| interruptible / cheapest compute      | **EC2 Spot**                           | 可中断省钱                 |
| long-term steady workload cost saving | **Savings Plans / RI**                 | 长期折扣                  |
| GPU inference/training                | **EC2 GPU**                            | GPU 主要在 EC2           |
| decouple / buffer / worker pulls      | **SQS**                                | 解耦削峰                  |
| fanout / one-to-many notify           | **SNS**                                | 广播推送                  |
| DB connection storm                   | **RDS Proxy**                          | 连接池复用                 |
| sub-millisecond reads / hot data      | **ElastiCache**                        | 内存缓存                  |
| read-heavy DB performance             | **RDS Read Replica**                   | 扩读吞吐                  |
| HA / automatic failover DB            | **RDS Multi-AZ**                       | 同步 standby + failover |

---

如果你要继续加速刷题，我可以再给你：

* **Compute 65 题整套模拟卷（带陷阱解析）**
* **Compute + Storage + Database 三域联动决策树**
* **“错题本模板（关键词→误选原因→秒杀规则）”**
