好，这是 **最后一天专用·最狠版本**。
**只考“成本陷阱”**，而且是 **VPC + LB + DB + Endpoint 四域混合**。
目标只有一个：**不因为“贵 / 多余 / 过度设计”丢分**。

---

# 🔥 30 题【成本陷阱】变态卷

**⏱️ 3 秒 / 题｜全是“看起来更稳但更贵”的坑**

> 规则：
>
> * 正确答案 = **满足需求的最低成本托管方案**
> * EC2 / NAT / Multi-AZ / Global / Interface Endpoint 经常是“过度设计”

---

## Q1–Q10（VPC / Endpoint / NAT 成本坑）

### Q1

私有子网 EC2 只访问 **S3**，题干强调 *reduce cost*
A. NAT Gateway
B. EC2 NAT Instance
C. **S3 Gateway Endpoint**
D. Internet Gateway

---

### Q2

私有子网 Lambda 访问 **DynamoDB**
A. NAT Gateway
B. Interface Endpoint
C. **DynamoDB Gateway Endpoint**
D. EC2 Proxy

---

### Q3

私有子网访问 **SSM / Secrets Manager**
A. NAT Gateway
B. **Interface Endpoint**
C. Gateway Endpoint
D. IGW

> 💣 坑点：不是 S3/DDB 就 **不能 Gateway**

---

### Q4

应用只需访问 **AWS 托管服务**，不访问互联网
A. NAT Gateway
B. IGW
C. **VPC Endpoints**
D. EC2 Bastion

---

### Q5

每个 AZ 都放了一个 NAT Gateway，只是为了访问 S3
A. 正确设计
B. **浪费钱（该用 Gateway Endpoint）**
C. 需要更多 NAT
D. 用 EC2 NAT 更便宜

---

### Q6

私有子网 EC2 访问 ECR 拉镜像
A. NAT Gateway
B. **ECR Interface Endpoint**
C. S3 Gateway Endpoint
D. EC2 镜像同步

---

### Q7

VPC 里只有一个服务需要访问公网
A. 给整个 VPC 上 NAT
B. **只给该子网配置 NAT 路由**
C. 用 EC2 Proxy
D. 开 IGW 给所有子网

---

### Q8

题干：*lowest cost + private access to S3*
A. NAT Gateway
B. Interface Endpoint
C. **Gateway Endpoint**
D. ALB

---

### Q9

两个 VPC 少量通信，流量不大
A. Transit Gateway
B. **VPC Peering**
C. NAT Gateway
D. EC2 VPN

---

### Q10

多账号、多 VPC、未来还会持续增加
A. VPC Peering Mesh
B. **Transit Gateway**
C. NAT Gateway
D. EC2 Hub

---

## Q11–Q20（LB / DB 成本陷阱）

### Q11

Web 应用 **HTTP/HTTPS**，不需要 static IP
A. NLB
B. **ALB**
C. EC2 Nginx
D. CloudFront

> 💣 NLB 比 ALB **贵**，且不必要

---

### Q12

只需要 **L4 TCP**，而且需要 static IP
A. ALB
B. **NLB**
C. CloudFront
D. API Gateway

---

### Q13

RDS 需要 **高可用**，无读压力
A. Read Replica
B. **Multi-AZ**
C. Aurora Global
D. 更大实例

---

### Q14

数据库 **读多写少**
A. Multi-AZ
B. **Read Replica**
C. Aurora Global
D. Cache only

---

### Q15

题干：*sub-millisecond latency*
A. 更大 RDS
B. Read Replica
C. **ElastiCache / DAX**
D. Aurora Global

---

### Q16

应用在 **单 Region**，不提 DR
A. Aurora Global
B. Multi-AZ
C. **单 Region RDS（按需）**
D. 跨区复制

> 💣 Global = 贵，没提 DR 别选

---

### Q17

需要 **跨 Region DR**，但 *RPO 不严格*
A. Aurora Global
B. **跨区 Read Replica / Snapshot**
C. Multi-AZ
D. Cache

---

### Q18

数据库访问模式稳定、低并发
A. DynamoDB
B. Aurora
C. **RDS on-demand**
D. ElastiCache

---

### Q19

只为 HA 而加 Read Replica
A. 正确
B. **错误（Multi-AZ 才是 HA）**
C. 看引擎
D. 都行

---

### Q20

题干未提性能问题，却选 Cache
A. 正确
B. **过度设计（浪费钱）**
C. Cache 永远好
D. 看心情

---

## Q21–Q30（四域混合终极坑）

### Q21

私有子网 Web + DB，只需内部访问
A. 公网 ALB
B. **内部 ALB**
C. NLB
D. EC2 Proxy

---

### Q22

API 需要鉴权、限流、低并发
A. ALB + EC2
B. **API Gateway（HTTP API）**
C. NLB
D. EC2 自建

---

### Q23

流量低、功能简单，却选全套微服务
A. 正确
B. **过度设计（成本陷阱）**
C. AWS 推荐
D. 必须拆

---

### Q24

偶发异步任务
A. EC2 常驻 worker
B. **Lambda + SQS**
C. ECS 长跑
D. Step Functions

---

### Q25

fan-out + 每个消费者流量低
A. 多个 SQS
B. **SNS + SQS**
C. Kinesis
D. EC2 转发

---

### Q26

事件路由规则复杂，但事件量小
A. SNS
B. **EventBridge**
C. SQS
D. EC2

---

### Q27

需要顺序，但吞吐不高
A. SQS Standard
B. **SQS FIFO**
C. Kinesis
D. SNS

---

### Q28

仅为了日志分析就上 EC2 ELK
A. 正确
B. **成本陷阱（CloudWatch / OpenSearch 托管）**
C. ELK 必须
D. 看规模

---

### Q29

题干没提公网，却选 CloudFront
A. 正确
B. **浪费钱**
C. CloudFront 万能
D. 默认选

---

### Q30

题干出现：*cost-effective / simplest solution*
A. 复杂多 AZ 架构
B. EC2 自建
C. **最小托管方案**
D. Global 架构

---

## ✅ 标准答案

```
1C 2C 3B 4C 5B
6B 7B 8C 9B 10B
11B 12B 13B 14B 15C
16C 17B 18C 19B 20B
21B 22B 23B 24B 25B
26B 27B 28B 29B 30C
```

---

# 🎯 90 分钟【最后一天】全真模考打法

## ⏱️ 节奏（严格）

* **0–15 min：12 题**（别回看）
* **15–30 min：25 题**
* **30–60 min：50 题**
* **60–75 min：做完 65**
* **75–90 min：只回看“成本陷阱题”**

---

## 💣 成本陷阱终极清单（考前 5 分钟）

* **只访问 S3 / DynamoDB → Gateway Endpoint（免费）**
* **没写 DR → 别选 Global / 跨区**
* **HA ≠ Read Replica**
* **延迟没提 → 别上 Cache**
* **HTTP ≠ NLB（贵）**
* **低流量 ≠ 微服务**
* **看到 EC2 自建 = 先否掉**

---

如果你愿意，我可以最后再给你一份：
👉 **《SAA-C03 考前 10 分钟 · 绝对不该选的选项清单》**
（直接告诉你：**哪些词一出现，这个选项就必错**）

你只回一句：**“给我最后清单”**
