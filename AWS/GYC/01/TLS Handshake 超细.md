下面这套是 **SAA-C03 考试“闭眼秒选”级别**的 HTTPS / TLS 内容，一次性给你 **机制 + 陷阱 + 对比 + 速记卡**。

---

## 🔐 一、TLS Handshake **超细版 ASCII（逐步）**

> 目标：**知道“谁发什么、为什么、考试考哪里”**

```text
[Client / Browser]
        |
        | ① ClientHello
        |   - 支持的 TLS 版本
        |   - Cipher Suites
        |   - Random (Client Random)
        v
[Server / ALB or NLB]
        |
        | ② ServerHello
        |   - 选择的 TLS 版本
        |   - 选择的 Cipher
        |   - Random (Server Random)
        |
        | ③ Certificate
        |   - 公钥证书（CA 签名）
        |
        | ④ ServerHelloDone
        v
[Client]
        |
        | ⑤ 验证证书
        |   - CA 是否可信
        |   - 域名是否匹配
        |   - 是否过期
        |
        | ⑥ 生成 Pre-Master Secret
        |   - 用服务器公钥加密
        |
        | ⑦ ClientKeyExchange
        |
        | ⑧ ChangeCipherSpec
        | ⑨ Finished（加密）
        v
[Server]
        |
        | ⑩ 用私钥解密 Pre-Master
        |    推导 Session Key
        |
        | ⑪ ChangeCipherSpec
        | ⑫ Finished（加密）
        v
================== 安全信道建立完成 ==================
        |
        | 🔒 后续 HTTP 数据 → HTTPS（对称加密）
```

### 🧠 考试只记这 3 点

* **非对称加密**：只用在 *握手阶段*
* **对称加密**：只用在 *正式传输*
* **TLS 终止点在哪？** = 决定后端是否还需要证书

---

## 🧪 二、5 道 **HTTPS 场景陷阱题（真题风格）**

### Q1

题干：

> Need to offload SSL from EC2 instances

✅ **ALB**
🔑 暗号：*SSL/TLS termination*

---

### Q2

题干：

> Client IP must be preserved and traffic is HTTPS

✅ **NLB（TLS pass-through）**
🔑 暗号：*preserve client IP*

---

### Q3

题干：

> Use AWS WAF with HTTPS traffic

✅ **ALB**
🔑 暗号：*WAF 只能挂 ALB / CloudFront*

---

### Q4

题干：

> Backend instances should not manage certificates

✅ **ALB**
🔑 暗号：*TLS terminate at load balancer*

---

### Q5

题干：

> Extremely low latency HTTPS traffic, no HTTP routing needed

✅ **NLB**
🔑 暗号：*performance > intelligence*

---

## 🧠 HTTPS 陷阱一句话

> **WAF / HTTP 语义 → ALB**
> **性能 / TCP / IP → NLB**

---

## ⚖️ 三、**ALB vs NLB 的 HTTPS 能力对比（考试版）**

| 维度             | **ALB**         | **NLB**        |
| -------------- | --------------- | -------------- |
| 工作层            | L7 (HTTP/HTTPS) | L4 (TCP/TLS)   |
| TLS 终止         | ✅ 支持            | ✅ 支持           |
| TLS 透传         | ❌               | ✅              |
| HTTP Header    | ✅ 可解析           | ❌              |
| Path / Host 路由 | ✅               | ❌              |
| WAF 集成         | ✅               | ❌              |
| Client IP      | ❌（默认）           | ✅              |
| 延迟             | 低               | **极低（sub-ms）** |
| 固定 IP          | ❌               | ✅              |

### 🧠 一句话选型

* **“懂 HTTP”** → ALB
* **“只管快”** → NLB

---

## 🧾 四、SAA-C03 **HTTPS / TLS 速记卡（可直接背）**

```text
【TLS 本质】
- 握手：非对称
- 传输：对称

【TLS 终止】
- 在 ALB：后端走 HTTP
- 在 NLB：TLS pass-through 或 terminate

【ALB 选型暗号】
- HTTPS + WAF
- Header / Path / Host
- SSL offload
- 微服务路由

【NLB 选型暗号】
- HTTPS + 极低延迟
- Preserve client IP
- Fixed IP
- TCP / TLS

【考试禁区】
- NLB + WAF ❌
- ALB + TCP routing ❌
```

---

## ⚡ 30 秒 HTTPS 决策树（必背）

```text
是否需要 HTTP 语义？
   └─ YES → ALB

是否需要 WAF？
   └─ YES → ALB

是否需要 client IP / 极低延迟？
   └─ YES → NLB

TLS 在哪终止？
   ├─ ALB → 后端不用证书
   └─ NLB → 可透传或终止
```

---

## 🎯 考试生存法则

* **HTTPS ≠ 一定选 ALB**
* **TLS ≠ HTTP**
* **先问：我要不要“看懂 HTTP”？**

---

如果你愿意，下一步我可以继续帮你👇

* 🔥 **HTTPS × CloudFront × ALB 三选一魔鬼题**
* 🧠 **TLS / SSL / ACM 高频暗号表**
* 📄 **Networking + Security 终极速记 PDF**

你直接回我 **A / B / C**，我立刻继续。
