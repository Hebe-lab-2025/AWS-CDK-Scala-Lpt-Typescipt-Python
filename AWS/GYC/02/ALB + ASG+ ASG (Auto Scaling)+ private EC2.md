好，这一轮我直接**给你可刷、可背、可压测的成品**，全部围绕 **LB + ASG + RDS**，目标只有一个：**看到关键词 → 3 秒内出答案**。

---

# 🔥 一、30 道「只看关键词秒选」LB 盲选题（不看题干）

> **规则**：
>
> * 你**只看粗体关键词**
> * 不读背景
> * 3 秒内选答案
> * 这是考试最真实的能力要求

---

### 🧠 Blind Round（30 题）

**Q1**
关键词：**HTTP/HTTPS、path-based routing、TLS termination**
👉 答案：✅ **ALB**

**Q2**
关键词：**millions of TCP connections、ultra-low latency、static IP**
👉 答案：✅ **NLB**

**Q3**
关键词：**Layer 7、host-based routing、microservices**
👉 答案：✅ **ALB**

**Q4**
关键词：**preserve client IP、UDP traffic**
👉 答案：✅ **NLB**

**Q5**
关键词：**WebSockets、long-lived connections**
👉 答案：✅ **ALB**

**Q6**
关键词：**gRPC、HTTP/2**
👉 答案：✅ **ALB**

**Q7**
关键词：**extreme performance、financial trading system**
👉 答案：✅ **NLB**

**Q8**
关键词：**WAF integration、SQL injection protection**
👉 答案：✅ **ALB**

**Q9**
关键词：**one static IP per AZ**
👉 答案：✅ **NLB**

**Q10**
关键词：**content-based routing**
👉 答案：✅ **ALB**

---

**Q11**
关键词：**scale EC2 automatically、traffic spikes**
👉 答案：✅ **ALB + ASG**

**Q12**
关键词：**instance health check、replace unhealthy instances**
👉 答案：✅ **ASG**

**Q13**
关键词：**graceful scaling、no downtime deployments**
👉 答案：✅ **ALB + ASG**

**Q14**
关键词：**stateless application**
👉 答案：✅ **ALB + ASG**

**Q15**
关键词：**sticky sessions needed**
👉 答案：⚠️ **ALB（但优先想“是否能无状态”）**

---

**Q16**
关键词：**high availability database、automatic failover**
👉 答案：✅ **RDS Multi-AZ**

**Q17**
关键词：**read-heavy workload、scale reads**
👉 答案：✅ **RDS Read Replica**

**Q18**
关键词：**disaster recovery、standby instance**
👉 答案：✅ **RDS Multi-AZ**

**Q19**
关键词：**offload read traffic**
👉 答案：✅ **Read Replica**

**Q20**
关键词：**write performance issue**
👉 答案：❌ **不是 Read Replica（考虑 instance size / Aurora）**

---

**Q21**
关键词：**HTTPS from clients、private EC2**
👉 答案：✅ **ALB in public subnet**

**Q22**
关键词：**backend instances in private subnet**
👉 答案：✅ **ALB + private EC2**

**Q23**
关键词：**zero downtime + HA + scalable**
👉 答案：✅ **ALB + ASG + Multi-AZ RDS**

**Q24**
关键词：**cost-sensitive + variable traffic**
👉 答案：✅ **ALB + ASG (Auto Scaling)**

**Q25**
关键词：**global users、latency-based routing**
👉 答案：⚠️ **Route 53 + ALB**

---

**Q26**
关键词：**TCP pass-through、no HTTP inspection**
👉 答案：✅ **NLB**

**Q27**
关键词：**application-layer routing logic**
👉 答案：✅ **ALB**

**Q28**
关键词：**millisecond matters**
👉 答案：✅ **NLB**

**Q29**
关键词：**WAF + HTTPS**
👉 答案：✅ **ALB**

**Q30**
关键词：**classic exam default for web apps**
👉 答案：✅ **ALB**

---

# 🧠 二、LB + ASG + RDS「一页终极决策树」

```text
                用户访问
                    |
            HTTP / HTTPS ?
               /        \
            YES          NO
             |            |
        需要内容路由?    TCP / UDP
           /     \           |
        YES       NO         |
         |         |         |
       ALB       ALB       NLB
         |
   是否需要自动扩容?
         |
        YES
         |
        ASG
         |
   数据库是否高可用?
        /   \
     YES     NO
      |       |
 RDS Multi-AZ  单实例
      |
 是否读压力大?
      |
     YES
      |
 Read Replicas
```

### 🧠 一句话考试口诀

> **Web 应用默认：ALB → ASG → RDS Multi-AZ**
> **读慢再加 Replica，HA 不是为性能**

---

# 🧪 三、90 分钟完整 Load Balancer 模拟小卷（结构说明）

> ⚠️ 我下一步可以**直接给你整套题目 PDF**

### 📄 模拟卷结构（共 35–40 题，90 分钟）

| 模块         | 题量 | 陷阱                    |
| ---------- | -- | --------------------- |
| ALB vs NLB | 12 | UDP / static IP / WAF |
| ALB + ASG  | 10 | sticky session 诱导     |
| RDS + LB   | 8  | Multi-AZ vs Replica   |
| 架构组合       | 5  | EC2 自建诱导              |
| 成本 / HA    | 5  | overengineering       |

每题附：

* **关键词定位**
* **3 秒秒杀理由**
* **最容易错的选项为什么错**

---

## 🎯 下一步你选一个（我立刻交付）

1️⃣ **把这 30 题做成 Anki 盲选卡**
2️⃣ **生成「LB 决策树」一页 PDF（考前 5 分钟）**
3️⃣ **直接出 90 分钟完整 LB 模拟卷（可打印）**

👉 你现在已经进入 **“条件反射层”**，
再刷 2–3 轮，LB 基本不会再丢分。
