好，我已经**帮你把 CSV 全部整理成「可读表格（Table）」版本**，方便你：

* 直接放 **Notion / Obsidian / GitHub Markdown**
* 人眼快速扫
* 后续一键再转回 Anki（Front / Back 很清晰）

下面是**纯结果版（不再解释）**👇

---

# 1️⃣ 30 题「LB 盲选」Anki 卡（Table 版）

> 规则：**只看关键词 → 3 秒内选答案**

| #  | Front（关键词）                                      | Back（秒选答案）               |
| -- | ----------------------------------------------- | ------------------------ |
| 1  | HTTP/HTTPS + path-based routing                 | ALB                      |
| 2  | millions of TCP connections + ultra-low latency | NLB                      |
| 3  | Layer 7 routing + host-based                    | ALB                      |
| 4  | UDP traffic + preserve client IP                | NLB                      |
| 5  | WebSocket + long-lived connections              | ALB                      |
| 6  | gRPC + HTTP/2                                   | ALB                      |
| 7  | extreme performance + trading system            | NLB                      |
| 8  | WAF integration required                        | ALB                      |
| 9  | static IP per AZ                                | NLB                      |
| 10 | content-based routing                           | ALB                      |
| 11 | traffic spikes + auto scale EC2                 | ALB + ASG                |
| 12 | replace unhealthy instances automatically       | ASG                      |
| 13 | zero downtime deployment                        | ALB + ASG                |
| 14 | stateless web application                       | ALB + ASG                |
| 15 | sticky sessions required                        | ALB（先考虑是否能无状态）           |
| 16 | high availability database                      | RDS Multi-AZ             |
| 17 | read-heavy workload                             | RDS Read Replica         |
| 18 | automatic failover                              | RDS Multi-AZ             |
| 19 | offload read traffic                            | Read Replica             |
| 20 | write performance issue                         | ❌ NOT Read Replica       |
| 21 | HTTPS from users + private EC2                  | ALB in public subnet     |
| 22 | backend in private subnet                       | ALB + private EC2        |
| 23 | HA + scalable + web app                         | ALB + ASG + RDS Multi-AZ |
| 24 | cost-sensitive + variable traffic               | ALB + ASG                |
| 25 | global users + latency routing                  | Route 53 + ALB           |
| 26 | TCP pass-through                                | NLB                      |
| 27 | application-layer routing                       | ALB                      |
| 28 | millisecond matters                             | NLB                      |
| 29 | WAF + HTTPS                                     | ALB                      |
| 30 | default choice for web apps                     | ALB                      |

📌 **Anki 转回 CSV 很简单**

* Front = 第 2 列
* Back = 第 3 列

---

# 2️⃣ 「LB + ASG + RDS」一页终极决策树（Table 心智版）

> 考前 5 分钟，用“判断路径”而不是记忆

| 判断点                         | YES          | NO        |
| --------------------------- | ------------ | --------- |
| 是否 HTTP / HTTPS？            | 继续           | NLB       |
| 是否需要 L7（path / host / WAF）？ | ALB          | ALB       |
| 是否需要自动扩容？                   | ASG          | 单 EC2     |
| 数据库是否要求高可用？                 | RDS Multi-AZ | Single DB |
| 是否读多写少？                     | Read Replica | 不加        |

### 一句话口诀

```
Web：ALB → ASG → RDS Multi-AZ  
性能 / TCP / UDP / 静态 IP：NLB  
HA ≠ 性能，读慢才加 Replica
```

---

# 3️⃣ 90 分钟 LB 模拟卷（结构速览 Table）

| 模块         | 题量 | 主要陷阱                  |
| ---------- | -- | --------------------- |
| ALB vs NLB | 12 | UDP / static IP / WAF |
| ALB + ASG  | 10 | sticky session 诱导     |
| RDS + LB   | 8  | Multi-AZ vs Replica   |
| 架构组合       | 5  | EC2 自建诱导              |
| 成本 / HA    | 5  | over-engineering      |

---

# 🎯 最终 LB 保命法则（Table 版）

| 看到什么                  | 立刻反射         |
| --------------------- | ------------ |
| 犹豫                    | 默认 ALB       |
| TCP / UDP / static IP | NLB          |
| 流量波动                  | ASG          |
| DB 高可用                | RDS Multi-AZ |
| 读慢                    | Read Replica |

---

## 👉 下一步你可以直接说一句

* **“把这些 Table 导出为 A4 / 锁屏 PDF”**
* **“继续下一模块（Database / VPC / Security）也用 Table + 盲选”**
* **“把整套做成 GitHub Repo 结构”**

你现在这套已经是 **考前可直接用版本** 了。
