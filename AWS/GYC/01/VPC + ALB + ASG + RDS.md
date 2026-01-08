## 1️⃣ VPC + ALB + ASG + RDS 完整三层考试图（Multi-AZ 标准答案）

```text
                        Internet
                           |
                        Route 53
                           |
                    Internet Gateway (IGW)
                           |
        ==================================================
        |                  VPC (10.0.0.0/16)            |
        ==================================================

   AZ-A (us-*-a)                               AZ-B (us-*-b)
   -------------------                         -------------------
   Public Subnet A                             Public Subnet B
   (10.0.1.0/24)                               (10.0.2.0/24)
     - ALB (HTTPS 443)                           - ALB nodes
     - NAT GW (optional)                         - NAT GW (optional)
     - Route: 0.0.0.0/0 -> IGW                   - Route: 0.0.0.0/0 -> IGW
           |                                             |
           | forwards (Target Group)                     | forwards
           v                                             v
   Private App Subnet A                          Private App Subnet B
   (10.0.11.0/24)                                (10.0.12.0/24)
     - EC2/ECS in ASG (Stateless)                  - EC2/ECS in ASG
     - No public IP                                - No public IP
     - Route (if outbound needed):                 - Route (if outbound needed):
         0.0.0.0/0 -> NAT GW                         0.0.0.0/0 -> NAT GW
           |                                             |
           | DB connect (3306/5432/...)                 | DB connect
           v                                             v
   Private DB Subnet A                           Private DB Subnet B
   (10.0.21.0/24)                                (10.0.22.0/24)
     - RDS Primary / Standby (Multi-AZ)            - RDS Standby / Primary
     - Publicly Accessible = NO                    - Publicly Accessible = NO
     - Use RDS Endpoint (NOT IP)                   - Failover swaps roles

Security controls (考试关键词)：
- SG(ALB): inbound 443 from 0.0.0.0/0; outbound to App SG
- SG(App): inbound from ALB SG; outbound to DB SG (port)
- SG(DB): inbound ONLY from App SG (port 3306/5432)
- NACL: 仅当题干强调“IP级阻断/显式允许”才重点检查（stateless）
```

---

## 2️⃣ VPC / NAT / SG / NACL 陷阱题 20 道（秒杀误选）

> 每题：**一句话题干 → 答案 → 1 句理由（暗号）**

1. **Private subnet 的 EC2 访问 RDS（同 VPC）**
   ✅ 不需要 NAT
   🔑 同 VPC 内网通信

2. **Private subnet EC2 `yum/apt` 失败**
   ✅ NAT Gateway（在 public subnet）
   🔑 outbound internet

3. **RDS 连接超时，最近只改了 SG**
   ✅ 查 DB SG inbound 端口
   🔑 90% 是 SG

4. **RDS Multi-AZ 后 failover 连接断**
   ✅ 用 RDS endpoint，不要写死 IP
   🔑 endpoint 才会跟随切换

5. **“Only allow specific IP ranges”**
   ✅ NACL
   🔑 IP 级、stateless

6. **“Only allow app servers to access DB”**
   ✅ DB SG 引用 App SG
   🔑 SG-to-SG 最标准

7. **把 ALB 放在 private subnet**
   ❌ 错（考试默认 ALB 需 public subnets）
   🔑 需要对公网入口

8. **EC2 有 public IP 但仍不能上网**
   ✅ 查 route table 是否指向 IGW
   🔑 public = 路由到 IGW

9. **RDS 设为 Publicly Accessible=YES 就一定能公网连**
   ❌ 不一定（还要 SG + subnet route）
   🔑 公网访问要三件套

10. **Ping 通 DB，但端口连不上**
    ✅ SG/NACL 端口规则问题
    🔑 ping ≠ 3306/5432

11. **“Need private access to S3 without internet”**
    ✅ VPC Gateway Endpoint for S3
    🔑 endpoint 省 NAT

12. **“Need private access to DynamoDB”**
    ✅ VPC Gateway Endpoint（DynamoDB）
    🔑 同上

13. **“Need inbound from internet to EC2”**
    ✅ public subnet + IGW + SG inbound
    🔑 inbound 靠 IGW+SG

14. **NAT Gateway 放在 private subnet**
    ❌ 错
    🔑 NAT 必须在 public subnet 才能出网

15. **“Block a single malicious IP quickly”**
    ✅ NACL（或 WAF 若在 L7 场景）
    🔑 NACL 更直接（题干说 IP）

16. **NACL 只配 inbound 不配 outbound**
    ❌ 错
    🔑 stateless 两边都要

17. **安全组规则改了但不生效（题干暗示 NACL 近期改动）**
    ✅ 查 NACL
    🔑 先看最近变更点

18. **RDS 在 private subnet 就“绝对安全”**
    ❌ 错（SG 仍可能放开）
    🔑 SG 才是门禁

19. **跨 AZ 通信需要 NAT**
    ❌ 错
    🔑 VPC 内跨 AZ 默认可路由

20. **“Need fixed IP for incoming traffic”**
    ✅ NLB（不是 ALB）
    🔑 固定 IP = NLB 暗号

---

## 3️⃣ 专练「为什么连不上 DB」场景题 10 道（逐步排查思维）

> 目标：把排查顺序练成肌肉记忆：
> **SG → 路由/子网 → NACL → Endpoint/DNS → 端口/应用层**

1. **EC2(Private) 连 RDS timeout，安全组没配**
   ✅ DB SG inbound 放开 App SG 到 3306/5432
   🔑 最典型

2. **EC2 能 ping RDS，但 MySQL 3306 timeout**
   ✅ 查 SG/NACL 端口 3306（ping 不代表 TCP 端口）

3. **RDS Multi-AZ 后“偶尔”连不上（切换后发生）**
   ✅ 检查应用是否写死了 IP；改用 endpoint

4. **RDS 在 private subnet，开发者从家里直连失败**
   ✅ 预期行为：private 不对公网；需 VPN/Direct Connect/Bastion（考试常见：不建议直接公网 DB）

5. **ALB 健康检查正常，但 App 报 DB 连接拒绝**
   ✅ DB SG 允许的是“某个 IP”，而不是 App SG（ASG IP 会变）→ 改 SG-to-SG

6. **最近加了 NACL 后 DB 全挂**
   ✅ NACL inbound/outbound 都要允许 DB 端口 + ephemeral ports（至少确保回包通）

7. **EC2 在另一个 VPC，想连 RDS**
   ✅ 需要 VPC Peering / Transit Gateway + 路由表更新 + SG 允许对端网段

8. **同 VPC 不同子网，仍连不上 DB**
   ✅ 查 route table 是否被误配（比如把私网路由指错 / 黑洞）

9. **RDS 端口正确，SG 也允许，但仍失败**
   ✅ 检查 DB 参数：监听端口/用户权限/数据库是否启动（考试会给“connection refused”暗示）

10. **“Could not resolve endpoint”**
    ✅ DNS/endpoint 配置问题：应用用错了 hostname（读写 endpoint 混用等）

---

## 4️⃣ SAA-C03 VPC 模拟小卷（15 题，含答案+一句话解析）

> 选项题（A/B/C/D）。你可以 30 秒一题连刷。

### Q1

Private subnet EC2 需要访问 internet 更新补丁，最佳方案？
A. IGW
B. NAT Gateway
C. VPC Peering
D. NACL
✅ **B** — outbound internet for private subnet

### Q2

要让全球用户更低延迟访问静态内容，HTTPS，选？
A. ALB
B. NLB
C. CloudFront
D. Route 53 only
✅ **C** — edge caching

### Q3

RDS 要高可用，自动故障转移，选？
A. Read Replica
B. Multi-AZ
C. Bigger instance
D. More IOPS only
✅ **B** — HA/failover

### Q4

“Only allow app servers to access DB” 应该怎么做？
A. DB 放 public subnet
B. DB SG inbound 允许 App SG
C. NACL 只开 inbound
D. NAT Instance
✅ **B** — SG-to-SG

### Q5

NACL 的特点是？
A. Stateful
B. Stateless
C. Only outbound
D. Only inbound
✅ **B** — inbound/outbound 都要配

### Q6

ALB 最适合的场景？
A. TCP 极低延迟
B. 固定 IP
C. Path/Host routing + WAF
D. UDP 游戏流量
✅ **C**

### Q7

NAT Gateway 应部署在哪？
A. Private subnet
B. Public subnet
C. DB subnet group
D. 任意 subnet 都行
✅ **B**

### Q8

RDS failover 后应用连不上，最常见原因？
A. 写死了 DB IP
B. 需要 NAT
C. 需要 IGW
D. 需要更大实例
✅ **A** — 要用 endpoint

### Q9

要从 private subnet 私有访问 S3，避免 NAT 成本，选？
A. Interface Endpoint
B. Gateway Endpoint
C. Peering
D. IGW
✅ **B** — S3/DynamoDB 常用 gateway endpoint

### Q10

Security Group 是？
A. Stateless
B. Stateful
C. IP-only control
D. Route table
✅ **B**

### Q11

“Block a specific IP range at subnet level” 选？
A. SG
B. NACL
C. Route 53
D. ALB rule
✅ **B**

### Q12

Public subnet 的定义最准确是？
A. 有 public IP
B. 路由表有 0.0.0.0/0 → IGW
C. 能访问 RDS
D. 有 NAT
✅ **B**

### Q13

EC2 在 public subnet，SG 正确，但仍无法入站访问，可能缺什么？
A. IGW
B. Route 53
C. VPC Endpoint
D. Read Replica
✅ **A**

### Q14

跨 VPC 访问私有资源，最直接方式之一是？
A. VPC Peering
B. NAT Gateway
C. CloudFront
D. Security Group 复制
✅ **A**

### Q15

题干出现 “preserve client IP + TLS + high performance” 更可能选？
A. ALB
B. NLB
C. CloudFront
D. API Gateway
✅ **B**

---

如果你想把第 4 部分做成**计时模式**（每题 3 秒、你回 A/B/C/D、我立刻判分并指出你错在哪个“暗号”），我也可以直接开始下一轮连刷。
