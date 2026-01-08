下面按**中文解释 + 术语拆解 + 考试点**来讲这一段（先讲场景，再讲 Step 1）。

---

# ✅ Case study 是什么？

**Case study** = **案例分析 / 情景题**
在 SAA-C03 里就是：给你一个真实业务场景，让你选最合适的 AWS 架构组合。

---

# ✅ three-tier web application（三层架构）是什么？

**three-tier** = **三层应用架构**，经典分层：

1. **Presentation / Web 层**：接收用户请求（UI/API 入口）
2. **Application 层**：业务逻辑（服务端计算）
3. **Data 层**：数据库/缓存/存储

考试里看到 three-tier，默认你要想到：
**入口层 + 计算层 + 数据层**，并且要考虑 **扩展、高可用、安全**。

---

# ✅ unpredictable spikes in traffic（不可预测的流量尖峰）

意思是：平时流量一般，但会突然暴涨（比如秒杀、活动、新闻爆火）。

考试暗示你要考虑：

* **Auto Scaling / 弹性**
* **负载均衡**
* **解耦（SQS）**
* **缓存（ElastiCache）**
* **按需计费（serverless/managed）**

---

# ✅ secure / highly available / cost-effective（安全 / 高可用 / 成本效率）

这是典型 AWS 三连关键词，分别触发：

* **secure**：WAF、TLS、最小暴露面、私有子网、安全组/NACL
* **highly available (HA)**：多 AZ、冗余、故障转移
* **cost-effective**：按需伸缩、托管服务、避免长期空转 EC2

---

# ✅ Step 1: Secure the entry point（第一步：保护入口）

**entry point** = 系统对外的“入口”（用户最先访问的地方）。
通常是：CloudFront / ALB / API Gateway。

---

# ✅ A common pitfall（常见坑）是什么？

**pitfall** = **常见陷阱 / 容易踩坑的做法**。

这里的坑是：

## ❌ exposing instances directly to the internet

**直接把 EC2 实例暴露到公网**（比如给 EC2 公网 IP，让用户直接打到实例上）

为什么是坑？

* 攻击面大（扫描、爆破、漏洞利用）
* 证书/TLS 管理复杂
* 难做高可用（单点）
* 难做统一防护（WAF 更适合放在入口）

---

# ✅ Application Load Balancer (ALB) 是什么？

**ALB** = 应用层负载均衡器（第 7 层，HTTP/HTTPS）

它做什么？

* 把用户请求分发到后端多个实例/服务
* 支持基于 **URL 路径/Host** 的路由（如 `/api` `/images`）
* 支持健康检查，自动剔除坏实例

考试暗号：

* Web 应用 HTTP/HTTPS → **优先想到 ALB**

---

# ✅ public subnets（公有子网）是什么意思？

**Public Subnet** = 路由表里有一条路由指向 **Internet Gateway (IGW)** 的子网。
放在这里的资源（例如 ALB）可以对公网提供服务。

考试常识：

* **ALB 通常放 public subnet**
* 后端 EC2/服务通常放 **private subnet**（私有子网）

---

# ✅ across at least two AZs（至少跨两个可用区）是什么意思？

**AZ (Availability Zone)** = 可用区（同一个 Region 内的独立机房/园区级隔离）。

“至少两个 AZ”是为了：

* **高可用**：一个 AZ 挂了，另一个还能扛
* SAA-C03 高频要求：**Multi-AZ**

---

# ✅ single point of contact（统一入口）是什么意思？

ALB 给用户一个固定入口（DNS），用户只需要访问 ALB：

* 不需要知道后端有多少台实例
* 后端扩容缩容不会影响用户入口
* 简化架构和运维

---

# ✅ offloads SSL/TLS termination（卸载 TLS 终止）是什么意思？

**SSL/TLS termination** = HTTPS 的加密“解密终止点”。

放在 ALB 上的好处：

* 证书集中管理（ACM 更方便）
* 后端实例不用各自处理 HTTPS
* 性能更好、配置更统一
* 后端可以走 HTTP 或内部加密（看合规要求）

考试常见表述：

* “terminate SSL at the load balancer”
  = **在负载均衡层做 HTTPS 终止**

---

# ✅ protecting backend instances from direct exposure（保护后端不直接暴露）

意思是：

* 用户只能访问 ALB
* 后端 EC2 在私有子网，没有公网 IP
* 安全组只允许 **来自 ALB 的流量** 进入

这是典型“最小暴露面”安全设计。

---

# ✅ AWS WAF 是什么？为什么 attach 到 ALB？

**AWS WAF (Web Application Firewall)** = Web 应用防火墙。

**attach**（挂载/关联）到 ALB 的作用：

* 在入口层就拦掉恶意请求
* 保护后端不被打到

WAF 能拦：

* SQL injection（SQL 注入）
* XSS（跨站脚本）
* 常见扫描/攻击模式
* 还可以做 rate limit（限速）

---

# ✅ SQL injection（SQL 注入）是什么？

攻击者把恶意 SQL 片段塞进输入参数，试图：

* 绕过登录
* 读/改数据库数据
* 删除表等

考试里提到 SQL injection，几乎就是在暗示：

* **WAF**
* **输入校验**
* **最小权限**
* **参数化查询**（应用层）

---

# 🧩 用一句话把 Step 1 说清楚（考试口径）

> 不要把实例直接暴露公网；应在至少两个 AZ 的公有子网放置 ALB，集中处理 HTTPS（TLS 终止）并作为统一入口，同时绑定 AWS WAF 抵御常见 Web 攻击（如 SQL 注入），从而保护后端资源。

---

如果你愿意，我可以继续把后续步骤也用同样方式展开，比如：

* Step 2：计算层怎么做 Auto Scaling / 私有子网
* Step 3：数据层怎么选 RDS Multi-AZ + 缓存
* Step 4：成本优化（Spot、缓存、S3、按需）
  并且给你画一张完整三层架构 ASCII 图。
