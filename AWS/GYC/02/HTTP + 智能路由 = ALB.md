
好，**这三件我按「直接可用」给你一次性交付内容**。  
你现在就能 **复制 → 导入 → 打印 → 开刷**。

---

# 1️⃣ 30 题「LB 盲选」Anki 闪卡（可直接导入）

> **用法重点**：**不看解析｜3 秒内选｜建立条件反射**

## 📥 Anki 导入格式（CSV）

> **字段顺序**：`Front,Back`


"**HTTPS** + **path-based routing**","**ALB**｜**Layer 7** + path/host routing"
"**Ultra low latency** + **static IP**","**NLB**｜**L4** + static IP"
"**WebSocket** support required","**ALB**｜supports WebSocket"
"**Preserve client IP**","**NLB**｜source IP preserved"
"**HTTP header inspection**","**ALB**｜can inspect headers"
"**Millions of requests per second**","**NLB**｜designed for **extreme scale**"
"**TLS termination** at load balancer","**ALB**｜**TLS offloading**"
"**TCP traffic only**","**NLB**｜**Layer 4**"
"**gRPC traffic**","**ALB**｜**HTTP/2 + gRPC**"
"**Cost-sensitive** + HTTP app","**ALB**｜cheaper than NLB"
"**UDP traffic**","**NLB**｜supports UDP"
"Need **WAF integration**","**ALB**｜**WAF only works with ALB**"
"**Lambda** as backend","**ALB**｜Lambda target supported"
"**Fixed IP** required","**NLB**｜**static IP per AZ**"
"**Host-based routing**","**ALB**｜host-based routing"
"**High throughput** + **low latency**","**NLB**｜optimized for performance"
"**HTTP redirects**","**ALB**｜supports redirects"
"**TLS pass-through**","**NLB**｜no TLS termination"
"Need **Layer 7 metrics**","**ALB**｜request-level metrics"
"**Legacy TCP app**","**NLB**｜best for TCP"
"Need **sticky sessions**","**ALB**｜supports stickiness"
"**Private IP** load balancing","**NLB**｜internal NLB"
"**HTTP/2** required","**ALB**｜supports HTTP/2"
"**Extreme performance spikes**","**NLB**｜handles sudden spikes"
"**Microservices routing**","**ALB**｜path-based routing"
"**Lowest latency possible**","**NLB**｜lower latency than ALB"
"**Application-level auth**","**ALB**｜**OIDC integration**"
"Need **access logs per request**","**ALB**｜detailed access logs"
"**Non-HTTP protocol**","**NLB**｜not HTTP-based"
"**Cost optimization** for web app","**ALB**｜default choice"


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
