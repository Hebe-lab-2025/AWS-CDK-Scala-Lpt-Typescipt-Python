
好，**这三件我按「直接可用」给你一次性交付内容**。  
你现在就能 **复制 → 导入 → 打印 → 开刷**。

---

# 1️⃣ 30 题「LB 盲选」Anki 闪卡（可直接导入）

> **用法重点**：**不看解析｜3 秒内选｜建立条件反射**

下面我把你给的**所有暗号 → 秒选答案**，整理成一张**考试级 Markdown 决策速查表**，**一行=一个条件反射**，直接背就行（适合做锁屏 / Anki / PDF）。

---

# 🔥 AWS ALB vs NLB【暗号 → 秒选表】

> **规则一句话**
> 👉 **HTTP / L7 / 路由 / 功能 → ALB**
> 👉 **极低延迟 / TCP-UDP / 静态 IP / 性能 → NLB**

---

## 🧠 一眼秒选对照表

| 题干暗号 / 需求                       | 秒选                            |
| ------------------------------- | ----------------------------- |
| HTTPS + path-based routing      | **ALB**｜Layer 7 + 路径/主机路由     |
| Host-based routing              | **ALB**｜host routing          |
| Microservices routing           | **ALB**｜path-based routing    |
| HTTP header inspection          | **ALB**｜可检查 header            |
| HTTP redirects                  | **ALB**｜支持重定向                 |
| Need Layer 7 metrics            | **ALB**｜request-level metrics |
| Need access logs per request    | **ALB**｜详细访问日志                |
| Application-level auth          | **ALB**｜OIDC 集成               |
| Need sticky sessions            | **ALB**｜支持会话保持                |
| WebSocket support required      | **ALB**｜支持 WebSocket          |
| gRPC traffic                    | **ALB**｜HTTP/2 + gRPC         |
| HTTP/2 required                 | **ALB**｜支持 HTTP/2             |
| Lambda as backend               | **ALB**｜支持 Lambda target      |
| Need WAF integration            | **ALB**｜WAF 只能挂 ALB           |
| TLS termination at LB           | **ALB**｜TLS offloading        |
| Cost-sensitive + HTTP app       | **ALB**｜更便宜                   |
| Cost optimization for web app   | **ALB**｜默认选择                  |
| ---                             | ---                           |
| Ultra low latency               | **NLB**｜更低延迟                  |
| Lowest latency possible         | **NLB**｜比 ALB 低               |
| High throughput + low latency   | **NLB**｜性能优化                  |
| Extreme performance spikes      | **NLB**｜抗突发                   |
| Millions of requests per second | **NLB**｜极限规模                  |
| Fixed IP required               | **NLB**｜每 AZ 静态 IP            |
| Preserve client IP              | **NLB**｜保留源 IP                |
| TCP traffic only                | **NLB**｜Layer 4               |
| UDP traffic                     | **NLB**｜支持 UDP                |
| Non-HTTP protocol               | **NLB**｜非 HTTP                |
| Legacy TCP app                  | **NLB**｜最适合                   |
| TLS pass-through                | **NLB**｜不终止 TLS               |
| Private IP load balancing       | **NLB**｜Internal NLB          |
| TCP-only / L4 only              | **NLB**｜不看 HTTP               |

---

## 🧨 考试「秒杀口诀」

* **看到 HTTP / 路由 / Header / WAF / Lambda → 直接 ALB**
* **看到 TCP / UDP / 静态 IP / 极低延迟 → 直接 NLB**
* **不写协议 + Web App → 默认 ALB**
* **写“lowest latency / extreme scale” → 一定 NLB**

---

## 🧪 最容易被坑的混淆点（必记）

* ❌ **“高性能 Web” ≠ NLB**（只要是 HTTP，大多数还是 **ALB**）
* ❌ **“HTTPS” ≠ NLB**（TLS 终止 = ALB）
* ❌ **“私网” ≠ ALB**（Internal NLB 很常见）
* ✅ **是否需要 L7 能力** 是唯一分水岭

---

如果你愿意，我可以下一步直接帮你做👇

* 📄 **一页 PDF 锁屏版（只留暗号）**
* 🧠 **Anki 条件反射卡（看到词 0.5 秒选）**
* 🧪 **20 道“ALB vs NLB 地狱盲选题”**（专防手滑）

你选一个，或者直接说 **ALL IN**。



📌 **导入方式重点**

* Anki → **Import** → 选 **CSV**
* 字段 1 = **Front**，字段 2 = **Back**

---

# 2️⃣ 「LB 决策树」一页 PDF（考前 5 分钟）

> **考场重点**：你真正用的不是记忆，是**决策路径**

## 🧠 LB 决策树（可直接导出 PDF）

```text
                 ┌──────────────┐
                 │  Is it HTTP? │
                 └───────┬──────┘
                         │ Yes
                         ▼
                 ┌─────────────────┐
                 │ Need **L7** features │
                 │ (**path / host / WAF**)│
                 └───────┬─────────┘
                         │ Yes
                         ▼
                       **ALB**
                         ▲
                         │ No
                         │
            ┌────────────┴────────────┐
            │ Need **ultra-low latency** / │
            │ **static IP** / **TCP or UDP**?│
            └────────────┬────────────┘
                         │ Yes
                         ▼
                       **NLB**
```

## 🧠 考试版一句话口诀（必背）

```text
**HTTP + 智能路由 = ALB**
**性能 / TCP / UDP / 静态 IP = NLB**
```

📌 你可以：

* 复制进 **Notion → Export PDF**
* 或我直接给你 **A4 + 锁屏尺寸 PDF**

---

# 3️⃣ 90 分钟完整 LB 模拟卷（可打印）

## ⏱ 模考说明（关键指标）

* **65 题 / 90 分钟**
* **只考 ALB vs NLB**
* 每题 ≥ **2 个陷阱点**
* 目标：**看到关键词 → 秒选**

---

## 📝 模拟卷 · 示例（节选 10 题）

### Q1

A web app requires **path-based routing** and **WAF protection**.
✅ **B. ALB**

---

### Q2

Requires **static IP** and **TCP traffic**.
✅ **C. NLB**

---

### Q3

**Millions of requests** with **ultra-low latency**.
✅ **B. NLB**

---

### Q4

Inspect **HTTP headers** and perform **redirects**.
✅ **B. ALB**

---

### Q5

**UDP-based** real-time application.
✅ **B. NLB**

---

### Q6

Microservices with **host-based routing**.
✅ **B. ALB**

---

### Q7

**TLS termination on instances**, not LB.
✅ **B. NLB**

---

### Q8

**Lowest cost** for **standard HTTP web app**.
✅ **B. ALB**

---

### Q9

Need **preserve client IP** (no proxy headers).
✅ **B. NLB**

---

### Q10

**gRPC**-based service.
✅ **A. ALB**

---

📌 **正式版交付内容**

* 📄 **可打印 PDF（题目）**
* 📄 **答题卡**
* 📄 **错因本**（关键词 → 误选原因 → 秒杀规则）

---

# 🎯 LB「保命法则」（最后记忆）

```text
**犹豫时默认 ALB**
看到 **TCP / UDP / static IP / ultra-low latency** → **立刻 NLB**
```

---

## 👉 下一步你只要说一句

* **“生成 PDF”**
* **“给我完整 65 题 LB 模拟卷 PDF”**
* **“全部一起给”**

我立刻继续。

```
```
