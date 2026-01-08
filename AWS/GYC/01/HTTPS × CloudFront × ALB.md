好，这一组是 **SAA-C03 网络 + 安全的“终极混淆区”**。
我按 **考试秒选视角** 给你：**魔鬼题 → 暗号表 → 一页速记（PDF 可直接生成）**

---

# 🔥 一、HTTPS × CloudFront × ALB

## **三选一魔鬼题（10 题，真题级）**

> 规则：**先抓“用户在哪 + HTTPS 在哪终止 + 要不要边缘”**

---

### Q1

题干：

> Global users, static + dynamic content, HTTPS required, low latency.

✅ **CloudFront + ALB**
🔑 暗号：*global + low latency*

---

### Q2

题干：

> HTTPS traffic needs **WAF protection** and **path-based routing**.

✅ **ALB**
🔑 暗号：*WAF + path*

---

### Q3

题干：

> Serve static files (images/videos) securely over HTTPS worldwide.

✅ **CloudFront**
🔑 暗号：*static + worldwide*

---

### Q4

题干：

> Dynamic API traffic, regional users, HTTPS termination only.

✅ **ALB**
🔑 暗号：*regional + API*

---

### Q5

题干：

> Reduce latency by caching content closer to users.

✅ **CloudFront**
🔑 暗号：*cache at edge*

---

### Q6

题干：

> Need custom domain + TLS cert, no backend logic.

✅ **CloudFront**
🔑 暗号：*edge HTTPS only*

---

### Q7

题干：

> Need host-based routing and microservices over HTTPS.

✅ **ALB**
🔑 暗号：*microservices + routing*

---

### Q8

题干：

> Protect application from DDoS and TLS attacks globally.

✅ **CloudFront**
🔑 暗号：*edge + shield*

---

### Q9

题干：

> HTTPS with **sub-millisecond latency**, TCP-level.

✅ **NLB（不是 ALB / CloudFront）**
🔑 暗号：*extreme performance*
⚠️ 这是**反直觉坑**

---

### Q10

题干：

> Users worldwide, HTTPS, WAF, backend is web app.

✅ **CloudFront → ALB**
🔑 暗号：*edge + intelligent routing*

---

## 🧠 三选一终极口诀

```
Edge / Global / Cache / DDoS  → CloudFront
HTTP 语义 / WAF / 路由        → ALB
极限性能 / TCP / IP           → NLB
```

---

# 🧠 二、TLS / SSL / ACM **高频暗号表（考试必背）**

## 🔐 TLS / SSL 本质

* **SSL = 旧称**
* **TLS = 现在标准**
* 考试中 **SSL/TLS 混用，意思一样**

---

## 🔑 TLS 握手暗号

* 非对称加密：**只在握手**
* 对称加密：**真正传输**
* HTTPS = HTTP + TLS

---

## 📜 ACM（AWS Certificate Manager）

| 暗号                    | 秒选             |
| --------------------- | -------------- |
| Free public cert      | ACM            |
| Auto renewal          | ACM            |
| ALB / CloudFront cert | ACM            |
| Private CA            | ACM Private CA |

❌ **ACM 证书不能导出私钥（考试会考）**

---

## 🔥 TLS 终止位置（高频）

| 场景         | 终止点               |
| ---------- | ----------------- |
| CloudFront | Edge              |
| ALB        | Load Balancer     |
| NLB        | 可 terminate / 可透传 |
| EC2        | Instance（最不推荐）    |

---

## ❌ 高频错误认知

| 错误                | 正解 |
| ----------------- | -- |
| HTTPS 一定用 ALB     | ❌  |
| CloudFront 只能配 S3 | ❌  |
| TLS = 对称加密        | ❌  |
| ACM 可导出私钥         | ❌  |

---

# 📄 三、Networking + Security **终极速记（PDF 一页版内容）**

> ⬇️ 下面这块 **可直接复制生成 PDF**

---

## 🧭 【网络 + 安全一页决策卡】

### ① HTTPS 选型

```
Global users?        → CloudFront
Need WAF / routing? → ALB
Extreme latency?    → NLB
```

---

### ② TLS / HTTPS

```
Handshake: 非对称
Transfer: 对称
TLS terminate 越前越省资源
```

---

### ③ CloudFront

* Edge caching
* Global acceleration
* Built-in DDoS protection
* HTTPS at edge
* 可接：S3 / ALB / EC2

---

### ④ ALB

* L7 HTTP/HTTPS
* Path / Host routing
* WAF
* TLS termination
* 微服务首选

---

### ⑤ NLB

* L4 TCP/TLS
* Sub-ms latency
* Preserve client IP
* Fixed IP
* 不支持 WAF

---

### ⑥ ACM

* 免费公有证书
* 自动续期
* 不可导出私钥
* CloudFront / ALB 标配

---

## ⚡ 30 秒终极决策树（闭眼背）

```text
是否全球用户？
  └─ YES → CloudFront

是否需要 HTTP 语义 / WAF？
  └─ YES → ALB

是否极限性能 / TCP？
  └─ YES → NLB

证书怎么管？
  └─ ACM
```

---

## 🎯 SAA-C03 生存法则（很重要）

* **CloudFront ≠ 只有静态**
* **HTTPS ≠ 一定 ALB**
* **TLS 终止越靠前越好**
* **ACM 是默认安全答案**

---

### 下一步我可以直接帮你（强烈推荐）👇

* 🔥 **Networking + Security “反直觉挖坑题”20 道**
* 🧠 **SAA-C03 全域 3 秒决策总表（最终版）**
* 📄 **直接生成可下载的 PDF（目录 + 封面）**

你回我 **A / B / C**，我立刻继续。
