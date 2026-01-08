下面这套是 **SAA-C03 / 面试 / 架构题** 都通吃的版本，我按「**一条主线 + 四个模块**」给你串清楚。

---

## 一、SSL / HTTPS / ALB **完整考试流程图（ASCII）**

```
[ Client / Browser ]
        |
        | ① HTTPS Request (TLS Handshake starts)
        v
+-------------------+
|   ALB (HTTPS)     |  ← 证书在这里 (ACM / Uploaded Cert)
|  - TLS Terminate  |
+-------------------+
        |
        | ② Decrypted HTTP
        v
+-------------------+
|   EC2 / ECS       |  ← 应用只处理 HTTP
|   App Server      |
+-------------------+
```

### 🔑 考试关键词

* **TLS Termination at ALB**
* **Certificate managed by ACM**
* **Offload SSL from EC2**
* **Layer 7 Load Balancer**

---

## 二、为什么 HTTPS 一定要「对称 + 非对称混合」？（必考原理）

### 1️⃣ 只用非对称加密 ❌（RSA / ECC）

* 加密慢
* CPU 消耗大
* **不适合大量数据传输**

### 2️⃣ 只用对称加密 ❌（AES）

* 加密快 ✅
* **但密钥怎么安全传？❌**

---

### ✅ HTTPS 的混合方案（考试答案）

```
非对称加密 → 用来“安全交换密钥”
对称加密   → 用来“高速传输数据”
```

### TLS 简化流程

```
1. Client: 我要 HTTPS
2. Server: 给你公钥 + 证书
3. Client: 用公钥加密一个【对称密钥】
4. Server: 用私钥解密
5. 后续通信：全部用对称加密（AES）
```

📌 **一句考试金句**

> HTTPS uses asymmetric encryption for key exchange and symmetric encryption for data transfer.

---

## 三、SSL at ALB vs SSL at EC2（高频对比题）

| 对比项  | SSL at ALB ✅   | SSL at EC2 ❌ |
| ---- | -------------- | ------------ |
| 证书管理 | ACM 自动续期       | 手动管理         |
| 性能   | 集中处理，省 EC2 CPU | EC2 被拖慢      |
| 扩展性  | ASG 无感扩容       | 每台都要配证书      |
| 安全   | 私钥不下发到实例       | 私钥在实例        |
| 推荐度  | ⭐⭐⭐⭐⭐（考试首选）    | ⭐            |

📌 **考试结论**

> Unless explicitly required, always terminate SSL at ALB.

---

## 四、10 道 SSL / HTTPS / ALB 高频考题（含秒杀答案）

### Q1

**Why use HTTPS instead of HTTP?**
✅ Encryption + Integrity + Authentication

---

### Q2

**Which AWS service manages SSL certificates for ALB?**
✅ AWS Certificate Manager (ACM)

---

### Q3

**Where should SSL termination happen for best performance?**
✅ Application Load Balancer

---

### Q4

**ALB works at which OSI layer?**
✅ Layer 7

---

### Q5

**Which encryption is used after TLS handshake?**
✅ Symmetric encryption (AES)

---

### Q6

**Why is asymmetric encryption not used for bulk data?**
✅ Too slow and CPU-intensive

---

### Q7

**Does HTTPS require SSL/TLS?**
✅ Yes (HTTPS = HTTP over TLS)

---

### Q8

**Can ALB handle HTTPS traffic?**
✅ Yes (HTTP / HTTPS only)

---

### Q9

**What happens to traffic between ALB and EC2 after SSL termination?**
✅ Plain HTTP (unless re-encrypted)

---

### Q10

**Best practice for scalable HTTPS web app?**
✅ HTTPS at ALB + HTTP to EC2 + ASG

---

## 五、考试终极记忆版（一句话）

> **ALB terminates HTTPS using ACM, offloads SSL, uses asymmetric encryption for key exchange and symmetric encryption for fast data transfer.**

---

如果你愿意，我可以下一步帮你：

* 🔹 画 **TLS Handshake 超细版 ASCII（逐步）**
* 🔹 出 **5 道“HTTPS 场景陷阱题”**
* 🔹 对比 **ALB vs NLB 的 HTTPS 能力**
* 🔹 把这套内容做成 **SAA-C03 速记卡**

你挑一个，我直接展开。
