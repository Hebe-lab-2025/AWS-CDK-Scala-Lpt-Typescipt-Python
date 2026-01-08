太好了，这一轮我直接给你 **SAA-C03「网络 × 数据库 × 条件反射」终极实战包**。
下面 **4 个部分全部给你成品**，按你列的顺序来，**考场可直接用**。

---

# 1️⃣ 🔥 再拆 **10 道 VPC + RDS 混合地狱题**（只考误判点）

> 规则：**每题只有 1 个真正问题，其余都是烟雾弹**

---

### Q1

**EC2 与 RDS 在同一 VPC、不同 Subnet，连接超时**
✅ **检查 RDS Security Group inbound**
❌ NAT / IGW
🔑 **同 VPC 默认可达，连不上 90% 是 SG**

---

### Q2

**RDS 在 private subnet，想从本地 laptop 连接调试**
✅ **Publicly Accessible + Public Subnet + SG**
❌ NAT Gateway
🔑 **NAT 不支持入站**

---

### Q3

**RDS Multi-AZ 发生 failover，应用断连**
✅ **使用 RDS Endpoint（不是 IP）**
❌ 增加实例规格
🔑 **网络没问题，是连接方式错**

---

### Q4

**Private EC2 能 ping RDS，但 SQL 连接失败**
✅ **端口未放行（SG / NACL）**
❌ 路由表
🔑 **Ping ≠ TCP 端口**

---

### Q5

**Private EC2 访问 S3 导致 NAT 费用暴涨**
✅ **S3 Gateway Endpoint**
❌ 加大 NAT
🔑 **AWS 服务 ≠ NAT**

---

### Q6

**跨 VPC 的 EC2 无法访问 RDS**
✅ **VPC Peering / Transit Gateway**
❌ NAT
🔑 **NAT 不解决 VPC 互通**

---

### Q7

**RDS CPU 100%，Web 层已 ASG 扩容**
✅ **Cache / Read Replica**
❌ 再加 ASG
🔑 **瓶颈在数据层**

---

### Q8

**RDS 只允许应用服务器访问**
✅ **DB SG 引用 App SG**
❌ NACL
🔑 **精确控制 → SG**

---

### Q9

**Private subnet EC2 无法 yum install，但 DB 正常**
✅ **NAT Gateway**
❌ Endpoint（除非 AWS 服务）
🔑 **这是公网访问问题**

---

### Q10（反直觉）

**小型 MySQL 应用，只要求高可用**
✅ **RDS Multi-AZ**
❌ Aurora
🔑 **HA ≠ 高性能**

---

## 🧠 一句话总结

```
连 DB → SG
上网 → NAT
访问 AWS → Endpoint
HA ≠ 性能
```

---

# 2️⃣ 🧪 **30 题「只看暗号不看题」盲选卷**

> **规则：你只看粗体关键词 → 直接秒选**

---

1. **high availability, relational** → **RDS Multi-AZ**

2. **read-heavy workload** → **Read Replica**

3. **sub-millisecond latency** → **ElastiCache**

4. **private subnet outbound only** → **NAT Gateway**

5. **private EC2 access S3, cost sensitive** → **Gateway Endpoint**

6. **temporary credentials on EC2** → **IAM Role**

7. **fan-out notifications** → **SNS**

8. **decouple background jobs** → **SQS**

9. **unpredictable traffic** → **ALB + ASG**

10. **no servers to manage** → **Lambda**

11. **block SQL injection** → **WAF**

12. **DDoS protection** → **Shield**

13. **shared filesystem** → **EFS**

14. **boot volume** → **EBS**

15. **object storage** → **S3**

16. **private access to CloudWatch** → **Interface Endpoint**

17. **access external API from private subnet** → **NAT**

18. **public vs private subnet** → **Route Table**

19. **instance-level firewall** → **Security Group**

20. **subnet-level firewall** → **NACL**

21. **stateless web tier** → **ALB + ASG**

22. **user session lost after scale-out** → **JWT / Redis**

23. **long-running task** → **EC2 / ECS**

24. **global low latency** → **CloudFront**

25. **extreme TCP performance** → **NLB**

26. **DB failover breaks app** → **Use Endpoint**

27. **reduce NAT cost for AWS APIs** → **Endpoint**

28. **read latency spike** → **Cache**

29. **analytics slowing DB** → **Read Replica**

30. **single EC2 for scalability** → ❌ **错**

---

# 3️⃣ 🎯 **真实考试模拟：90 分钟 → 暗号 → 秒选**

## ⏱️ 时间策略（照做）

```
0–15 分钟：快速通读，直接秒选 60%
15–70 分钟：只回看“不确定题”
70–90 分钟：检查有没有选 EC2 / NAT / Aurora 这种诱导项
```

---

## 🧠 每道题的 3 步微流程（考场用）

```
① 划关键词（scalable / HA / private / latency）
② 翻译成暗号
③ 套用条件反射表
```

### 示例

题干出现：

> private subnet + access S3 + cost

→ 翻译：**AWS 服务 + 私网 + 成本**
→ 秒选：**Gateway Endpoint**

---

# 4️⃣ 🧠 **一页「条件反射表」（终极版）**

> **这页就是你脑中的 lookup table**

---

## 🟦 Compute

| 暗号                    | 秒选        |
| --------------------- | --------- |
| unpredictable traffic | ALB + ASG |
| no servers            | Lambda    |
| long-running task     | EC2 / ECS |
| stateless web         | ALB + ASG |

---

## 🟨 Storage

| 暗号                | 秒选      |
| ----------------- | ------- |
| object storage    | S3      |
| shared filesystem | EFS     |
| boot disk         | EBS     |
| archive           | Glacier |

---

## 🟩 Database

| 暗号          | 秒选           |
| ----------- | ------------ |
| HA only     | Multi-AZ     |
| read-heavy  | Read Replica |
| low latency | Cache        |
| failover    | Endpoint     |

---

## 🟥 Networking

| 暗号                 | 秒选          |
| ------------------ | ----------- |
| outbound only      | NAT         |
| AWS service access | Endpoint    |
| public vs private  | Route Table |
| block IP           | NACL        |
| app-only access    | SG          |

---

## 🟪 Security

| 暗号                    | 秒选       |
| --------------------- | -------- |
| temporary credentials | IAM Role |
| SQL injection         | WAF      |
| DDoS                  | Shield   |

---

## 🎯 终极一句话（考场必念）

> **SAA-C03 不是选“最强方案”，而是避开“不可能方案”。**

---

### 如果你愿意，下一步我还能继续：

* 🔥 **全真 65 题 SAA-C03 综合压力模拟（限时）**
* 📄 **把这整套做成 PDF / Notion / GitHub 版本**
* 🧠 **暗号 → 秒选 Anki 闪卡（可导入）**

你直接回我 **A / B / C** 或 **“全部”**。
