下面给你 **SAA-C03 考试专用版本**，全部是 **“读题→秒选”** 训练材料，可直接存 GitHub / Notion。

---

# 🔥 Part 1：15 道 **ALB vs NLB** 陷阱题（真题风格）

> 规则：
>
> * **先看触发词 → 再选服务**
> * 每题后面给 **一句话判题理由（考试口径）**

---

### Q1

应用需要 **HTTP header / path-based routing**
**答案：ALB**
👉 只有 ALB 工作在 **Layer 7**

---

### Q2

要求 **sub-millisecond latency**，协议是 TCP
**答案：NLB**
👉 亚毫秒 + TCP = **NLB**

---

### Q3

客户端 IP 必须被后端实例直接看到
**答案：NLB**
👉 NLB 默认 **保留源 IP**

---

### Q4

Web 应用，需要根据 `/api` `/images` 转发
**答案：ALB**
👉 Path-based routing = ALB

---

### Q5

要支持 **WebSocket**
**答案：ALB**
👉 WebSocket 属于 L7

---

### Q6

极高并发，百万级 TPS，协议 TCP
**答案：NLB**
👉 极端性能场景 = NLB

---

### Q7

需要和 **AWS WAF** 集成
**答案：ALB**
👉 WAF 只能挂 ALB / CloudFront

---

### Q8

应用是 gRPC
**答案：ALB**
👉 gRPC = HTTP/2 = L7

---

### Q9

负载均衡一个 **非 HTTP 服务（自定义 TCP 协议）**
**答案：NLB**

---

### Q10

希望负载均衡器本身有 **固定 IP**
**答案：NLB**
👉 NLB 支持 **Elastic IP**

---

### Q11

根据 **Host header（域名）** 转发
**答案：ALB**

---

### Q12

TLS 终止 + HTTP header 注入
**答案：ALB**

---

### Q13

数据库前面做负载均衡（极少见但考试会考）
**答案：NLB**
👉 DB = TCP

---

### Q14

“Low latency + extreme performance”
**答案：NLB**
👉 这是 NLB 经典暗号

---

### Q15

“Intelligent routing for microservices”
**答案：ALB**
👉 Intelligent routing = L7

---

## 🧠 一句话口诀（直接背）

> **HTTP / Path / Header / WAF / WebSocket → ALB**
> **TCP / UDP / 极低延迟 / 固定 IP → NLB**

---

# 🧩 Part 2：ELB 全家桶 **一页速记脑图（Markdown）**

```text
                    ELB (Elastic Load Balancing)
                             |
        ------------------------------------------------
        |                      |                       |
      ALB                    NLB                    CLB
(Application LB)       (Network LB)        (Classic LB - legacy)
        |                      |
   Layer 7               Layer 4
   HTTP / HTTPS          TCP / UDP / TLS
        |                      |
 Path / Host routing    Ultra-low latency
 Header-based routing   Preserve client IP
 WAF supported          Static IP (EIP)
 WebSocket / gRPC       Millions TPS
```

---

### 🚫 考试中 **CLB = 不选**

> 除非题目明确写 **“existing legacy application using CLB”**

---

# ⚡ Part 3：SAA-C03 **负载均衡 3 秒决策树**

```text
【Step 1】题目协议？
   ├─ HTTP / HTTPS / gRPC / WebSocket → ALB
   └─ TCP / UDP / TLS → NLB

【Step 2】是否出现这些词？
   ├─ path / host / header / WAF → ALB
   ├─ sub-millisecond / ultra-low latency → NLB
   ├─ fixed IP / preserve client IP → NLB

【Step 3】性能 vs 智能？
   ├─ 智能路由 / 微服务 → ALB
   └─ 极限性能 / 网络层 → NLB
```

---

## 🎯 考试实战技巧（非常重要）

* **不要自己设计架构**
* **题干已经给答案暗号**
* 你只需要做一件事：

> **“看到关键词 → 匹配服务 → 秒选”**

---

如果你愿意，下一步我可以直接帮你做👇

* ✅ **20 道 ELB + Auto Scaling 联动陷阱题**
* ✅ **ALB vs CloudFront vs API Gateway 三选一魔鬼题**
* ✅ **SAA-C03 Load Balancer 全套 PDF（可打印）**
* ✅ **30 秒口述版考试速记稿（适合临考）**

你选一个，我立刻继续。
