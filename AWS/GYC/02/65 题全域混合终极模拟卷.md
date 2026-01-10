# 1️⃣ SAA-C03「65 题全域混合终极模拟卷」（终极版）

> **使用方法（90 分钟）**：
>
> * 先做题：只选 A/B/C/D（不看答案）
> * 做完再对答案 + 看“秒杀理由”
> * 每题目标：**定位暗号 → 3 秒出服务**

---

## ✅ 题目（Q1–Q65）

### Q1

公司要给 Web App 做 **HTTPS 终止**，并按 **Path** 把流量转发到不同 Target Group。
A. NLB
B. ALB
C. GWLB
D. CloudFront
**答案：B**

### Q2

需要 **静态 IP**、**TCP/UDP**、超低延迟负载均衡。
A. ALB
B. NLB
C. Classic LB
D. Route 53
**答案：B**

### Q3

要防护 **SQLi/XSS**，并基于规则拦截 Web 攻击。
A. Shield Advanced
B. WAF
C. GuardDuty
D. Macie
**答案：B**

### Q4

私网 EC2 需要访问公网下载补丁，最常见方案（不涉及 S3）。
A. IGW
B. NAT Gateway
C. VPC Peering
D. Transit Gateway
**答案：B**

### Q5

访问 **S3** 且希望省 NAT 成本、走 AWS 内网。
A. NAT Gateway
B. IGW
C. S3 Gateway Endpoint
D. Interface Endpoint to S3
**答案：C**

### Q6

需要从 VPC 私网访问 **Secrets Manager**，不走公网。
A. Gateway Endpoint
B. Interface Endpoint (PrivateLink)
C. NAT Gateway
D. VPC Peering
**答案：B**

### Q7

要求数据库 **高可用自动故障转移**。
A. RDS Read Replica
B. RDS Multi-AZ
C. Aurora Serverless v2
D. DynamoDB Global Tables
**答案：B**

### Q8

数据库读压力大，想 **分担读请求**。
A. Multi-AZ
B. Read Replica
C. 增大实例
D. 启用 WAF
**答案：B**

### Q9

用户要求 **亚毫秒级读取**，数据在 DynamoDB。
A. 增大 DynamoDB RCU
B. DynamoDB Streams
C. DAX
D. ElastiCache Redis（必须）
**答案：C**

### Q10

Web 层突发流量，想让应用层自动水平扩容。
A. ALB + ASG
B. NLB + RDS
C. Route 53 + S3
D. CloudFront + EBS
**答案：A**

### Q11

需要**事件驱动**：S3 上传对象后触发处理，无需轮询。
A. SQS
B. EventBridge
C. SNS
D. Kinesis Data Streams
**答案：B**

### Q12

要**解耦** Web Tier 与 Worker Tier，缓冲尖峰。
A. SNS
B. SQS
C. Kinesis
D. Step Functions（必须）
**答案：B**

### Q13

要把一条消息广播给多个订阅者（fan-out）。
A. SQS FIFO
B. SNS
C. EventBridge（必须）
D. Kinesis Firehose
**答案：B**

### Q14

强需求：**严格顺序 + exactly-once**。
A. SQS Standard
B. SNS
C. SQS FIFO
D. Kinesis
**答案：C**

### Q15

想把静态网站托管、低成本、全球加速。
A. EC2 + EBS
B. S3 Static Website + CloudFront
C. EFS
D. RDS
**答案：B**

### Q16

EBS 卷要加密，且希望最少改动。
A. 直接对现有未加密 EBS 启用加密（就地）
B. 创建快照 → 复制快照并加密 → 新卷替换
C. 只能重建实例
D. 只能用 KMS 自建
**答案：B**

### Q17

需要跨 AZ 的共享文件系统给多台 EC2 用。
A. EBS
B. EFS
C. Instance Store
D. S3 Glacier
**答案：B**

### Q18

Windows 应用需要 SMB 文件共享。
A. EFS
B. FSx for Windows File Server
C. S3
D. EBS
**答案：B**

### Q19

想用对象存储做长期归档，最低成本，允许小时级恢复。
A. S3 Standard
B. S3 Glacier Flexible Retrieval
C. EFS IA
D. EBS Cold HDD
**答案：B**

### Q20

跨 Region 灾备：要求 RPO 接近 0、写入双活。
A. S3 CRR
B. DynamoDB Global Tables
C. RDS Read Replica
D. EBS Snapshot
**答案：B**

### Q21

需要“最小权限 + 临时凭证”给 EC2 访问 S3。
A. Access Key 写入代码
B. IAM Role for EC2
C. IAM User attach policy
D. Root key
**答案：B**

### Q22

安全团队要求：禁止 S3 公网访问，且可以一键阻断。
A. Bucket ACL
B. Security Group
C. S3 Block Public Access
D. KMS
**答案：C**

### Q23

要检测异常 API 调用（可疑访问、异常地理位置）。
A. CloudTrail
B. GuardDuty
C. Config
D. Inspector
**答案：B**

### Q24

想记录“谁在什么时间调用了哪些 AWS API”。
A. CloudWatch Logs
B. CloudTrail
C. X-Ray
D. Cost Explorer
**答案：B**

### Q25

合规要求：持续评估资源是否符合规则（比如 S3 必须加密）。
A. Trusted Advisor
B. AWS Config
C. CloudTrail
D. Kinesis
**答案：B**

### Q26

一个账户有多个 VPC，需要中心化互联，规模大。
A. VPC Peering 网状互联
B. Transit Gateway
C. IGW
D. NAT Instance
**答案：B**

### Q27

两 VPC 互联，简单、点对点、不可传递。
A. Transit Gateway
B. VPC Peering
C. VPN
D. Direct Connect
**答案：B**

### Q28

需要从本地机房到 AWS 专线、稳定低延迟。
A. VPN
B. Direct Connect
C. IGW
D. Peering
**答案：B**

### Q29

想用加密隧道快速连 AWS（互联网之上）。
A. Direct Connect
B. Site-to-Site VPN
C. Peering
D. NAT Gateway
**答案：B**

### Q30

私网实例要被 ALB 访问，安全组最关键的是：
A. 允许 0.0.0.0/0 入站
B. 只允许 ALB SG 入站到应用端口
C. 只改 NACL
D. 只改路由表
**答案：B**

### Q31

要隔离子网级无状态规则，支持显式 deny。
A. Security Group
B. NACL
C. IAM Policy
D. Route Table
**答案：B**

### Q32

应用需要缓存，读多写少，降低数据库压力。
A. EBS
B. ElastiCache Redis
C. EFS
D. Glacier
**答案：B**

### Q33

容器服务：不想管服务器，按容器运行。
A. ECS on EC2
B. ECS on Fargate
C. EKS on EC2
D. Lambda
**答案：B**

### Q34

函数计算：事件触发、无需服务器。
A. ECS
B. EC2
C. Lambda
D. Batch
**答案：C**

### Q35

需要蓝绿/金丝雀发布，最常见在 L7。
A. ALB Target Groups + weighted routing（或 CodeDeploy）
B. NLB only
C. Route table
D. EBS snapshot
**答案：A**

### Q36

要做全球 DNS 级别的 failover / latency routing。
A. CloudFront
B. Route 53
C. ALB
D. API Gateway
**答案：B**

### Q37

CloudFront 的主要价值关键词：
A. block public access
B. global edge caching / acceleration
C. synchronous replication
D. database failover
**答案：B**

### Q38

应用跨多 AZ，需要自动替换坏实例。
A. Launch Template only
B. ASG + health check
C. AMI
D. EBS
**答案：B**

### Q39

想把服务器日志集中存储并可查询分析。
A. S3 + Athena（或 OpenSearch）
B. EBS only
C. Route 53
D. KMS only
**答案：A**

### Q40

突然出现大量 4xx/5xx，想追踪调用链。
A. CloudTrail
B. X-Ray
C. Config
D. GuardDuty
**答案：B**

### Q41

要在不改应用的情况下做 DDoS 基础防护。
A. WAF
B. Shield Standard
C. GuardDuty
D. Inspector
**答案：B**

### Q42

要把大量数据持续写入数据湖，几乎无需运维。
A. Kinesis Data Streams + 消费者自写
B. Kinesis Firehose 直投 S3
C. SNS
D. SQS FIFO
**答案：B**

### Q43

跨账号共享某个 AWS 服务访问（私网），最常见：
A. VPC Peering
B. PrivateLink (Interface Endpoint)
C. IGW
D. NAT Instance
**答案：B**

### Q44

数据库迁移上云，尽量不停机（最小停机）。
A. Snowball
B. DMS + CDC
C. S3 Transfer Acceleration
D. CloudFront
**答案：B**

### Q45

大规模离线数据搬运到 AWS（TB/PB），物理设备。
A. VPN
B. Snowball
C. DMS
D. DataSync
**答案：B**

### Q46

跨 Region 复制 S3 对象用于 DR。
A. S3 CRR
B. EFS replication
C. RDS Multi-AZ
D. VPC Peering
**答案：A**

### Q47

RDS 备份“用户误删数据”常用：
A. Multi-AZ
B. Automated backups / PITR
C. Read Replica
D. CloudFront
**答案：B**

### Q48

要实现“密钥管理 + 数据静态加密”，AWS 原生：
A. KMS
B. WAF
C. SNS
D. Shield
**答案：A**

### Q49

想限制某资源只能被特定条件访问（如源 IP / MFA）。
A. IAM Policy 条件
B. Security Group
C. NACL
D. Route 53
**答案：A**

### Q50

应用需要把用户上传的图片异步生成缩略图。
A. 直接在 Web EC2 同步处理
B. SQS + Lambda/Worker
C. 只用 ALB
D. 只扩容 RDS
**答案：B**

### Q51

要保证对象不可被删除或覆盖（合规留存）。
A. S3 Versioning
B. S3 Object Lock (WORM)
C. Glacier
D. EFS
**答案：B**

### Q52

多账户统一审计与合规检查的集中点（常见组合）。
A. CloudTrail + Config 聚合
B. X-Ray
C. Shield
D. Snowball
**答案：A**

### Q53

要给 ECS/EC2 提供“应用配置参数”，避免写死。
A. S3 Public
B. SSM Parameter Store / Secrets Manager
C. Route 53 TXT
D. WAF
**答案：B**

### Q54

需要跨 AZ 高吞吐共享块存储给多台实例。
A. EBS Multi-Attach（特定场景）
B. EFS
C. Instance Store
D. Glacier
**答案：A**

### Q55

需要把对象变化通知多个系统，同时还要解耦。
A. SNS → 多订阅（或 SNS→SQS fan-out）
B. SQS FIFO only
C. NAT Gateway
D. CloudWatch Alarm
**答案：A**

### Q56

想做“服务器无状态”，会话不放本机。
A. 只开 sticky sessions
B. Session 放 ElastiCache/DynamoDB
C. 只加大 EC2
D. 只开 NLB
**答案：B**

### Q57

跨 Region 读多写少，想就近读（数据库层）。
A. RDS Read Replica（跨 Region）
B. Multi-AZ
C. NLB
D. SQS
**答案：A**

### Q58

需要对 S3 数据分类识别敏感信息（PII）。
A. GuardDuty
B. Macie
C. Inspector
D. WAF
**答案：B**

### Q59

要给应用自动打补丁/安全基线扫描（主机层）。
A. Inspector
B. Shield
C. SNS
D. Route 53
**答案：A**

### Q60

要在不暴露公网的情况下访问 API（私网 API）。
A. Public API Gateway
B. Private API Gateway + VPC Endpoint
C. ALB only
D. IGW
**答案：B**

### Q61

想把 CloudWatch 指标异常触发通知到邮箱。
A. CloudWatch Alarm → SNS
B. SNS → CloudTrail
C. GuardDuty → WAF
D. Config → NAT
**答案：A**

### Q62

要实现“只允许公司办公网 IP 访问控制台”。
A. IAM 条件 + SourceIp
B. Security Group
C. NACL
D. Route table
**答案：A**

### Q63

想在 VPC 内部做东西向流量检查/防火墙式转发。
A. GWLB
B. ALB
C. NLB
D. CloudFront
**答案：A**

### Q64

需要“高并发写 + 全托管 + 自动扩展”，且 key-value。
A. RDS MySQL
B. DynamoDB
C. EFS
D. Redshift
**答案：B**

### Q65

想让 S3 通过 HTTPS 给外部安全访问，同时可自定义域名。
A. S3 + CloudFront + ACM
B. EBS + ALB
C. EFS + NLB
D. Glacier + Route 53
**答案：A**

---

# 2️⃣ 只刷「HTTPS / VPC / IAM」反直觉错题（高频坑 20 题）

> 每题给你：**暗号 → 正解 → 最容易错的点**

### T1（HTTPS）

暗号：**need TLS termination + WAF**
✅ ALB + ACM + WAF
❌ NLB（NLB 不能直接配 WAF）

### T2（HTTPS）

暗号：**end-to-end encryption required**
✅ ALB/NLB 都可，但要 **TLS 到后端**（后端也证书）
❌ 只在 ALB 终止 TLS（不满足端到端）

### T3（HTTPS）

暗号：**S3 自定义域名 + HTTPS**
✅ CloudFront + ACM（在 CloudFront）
❌ S3 静态站点直接挂 ACM（做不到）

### T4（HTTPS）

暗号：**redirect HTTP→HTTPS**
✅ ALB listener rule redirect
❌ NLB（L4 不做重定向）

---

### T5（VPC）

暗号：**private subnet instances need internet access**
✅ NAT Gateway（在 public subnet）
❌ IGW（IGW 不是给私网实例直接用的）

### T6（VPC）

暗号：**S3 access from private subnet, save NAT cost**
✅ S3 Gateway Endpoint
❌ NAT Gateway（更贵）

### T7（VPC）

暗号：**access Secrets Manager privately**
✅ Interface Endpoint
❌ Gateway Endpoint（只有 S3/DynamoDB）

### T8（VPC）

暗号：**NACL vs SG**
✅ NACL：stateless + subnet + explicit deny
❌ SG：stateful + instance level

### T9（VPC）

暗号：**VPC peering transitive routing**
✅ 不支持（要 TGW）
❌ “A peer B，B peer C，所以 A 到 C”——错

### T10（VPC）

暗号：**need central hub for many VPCs**
✅ Transit Gateway
❌ 大量 VPC Peering（爆炸）

---

### T11（IAM）

暗号：**temporary credentials for EC2**
✅ IAM Role (Instance Profile)
❌ Access Key 写到配置文件

### T12（IAM）

暗号：**least privilege + conditions**
✅ IAM policy condition（MFA, SourceIp, VPC endpoint）
❌ 只靠 Security Group（那是网络层）

### T13（IAM）

暗号：**deny public S3 quickly**
✅ S3 Block Public Access（账户/桶级）
❌ 只改 ACL（容易漏）

### T14（IAM）

暗号：**cross-account access best practice**
✅ AssumeRole + trust policy
❌ 共享一个 IAM User

### T15（IAM）

暗号：**permission boundary**
✅ 限制角色/用户最大权限上限
❌ 以为是“授予权限”（不是，是“上限封顶”）

### T16（IAM）

暗号：**service-to-service auth**
✅ Role for service（Lambda/EC2/ECS Task Role）
❌ 用户凭证

### T17（IAM + VPC）

暗号：**restrict S3 only from VPC endpoint**
✅ IAM condition: aws:sourceVpce
❌ 只设置 SG/NACL（拦不住 S3 公网路径）

### T18（HTTPS + IAM）

暗号：**CloudFront signed URL**
✅ 控制私有内容分发
❌ WAF（WAF 是防攻击，不是授权）

### T19（VPC）

暗号：**Interface Endpoint costs**
✅ 有 ENI + 每小时 + 流量费
❌ 以为永远比 NAT 便宜（不一定）

### T20（HTTPS）

暗号：**ACM cert region trap**
✅ CloudFront 用的证书通常要在 **us-east-1**（经典陷阱）
❌ 在任意 region 都行（会踩坑）

---

# 3️⃣ 90 分钟全真模考 + 错因拆解（直接可用）

## ⏱️ 考试节奏（90 分钟）

* **0–5 min**：快速浏览 65 题，标记你薄弱域（Networking/Security/DB）
* **5–70 min**：第一遍做完所有题（平均 **1 分钟/题**）
* **70–85 min**：只回看“犹豫题 / 标记题”（不要重做全卷）
* **85–90 min**：填涂检查（模拟真实交卷）

## ✅ 错因拆解模板（复制就能用）

每道错题只写 3 行，越短越有效：

```text
Q#：
触发暗号（题里哪句话）：
我为什么会选错（错误直觉）：
3秒秒杀规则（下次看到就选什么）：
```

## 🧨 最常见 8 类“错因”对照表（直接定位你是哪种）

1. **HA vs 性能混淆**：Multi-AZ（HA）≠ Read Replica（读性能）
2. **NAT vs Endpoint**：访问 AWS 服务优先 Endpoint（省钱+私网）
3. **SNS vs SQS**：广播=SNS；解耦缓冲= SQS
4. **WAF vs Shield**：WAF 防应用层；Shield 防 DDoS
5. **SG vs NACL**：SG 有状态；NACL 无状态+显式 deny
6. **CloudFront vs ALB**：加速缓存=CloudFront；七层路由=ALB
7. **IAM Role vs User key**：AWS 上跑的服务几乎都用 Role
8. **“自建更灵活”陷阱**：考试优先托管服务（Managed > DIY）

---

如果你要把这三部分做成**一页速记卡**（LB+ASG+RDS + Messaging + HTTPS/VPC/IAM），我也可以直接把“暗号→秒选”压缩成 **考前 5 分钟版本**。
