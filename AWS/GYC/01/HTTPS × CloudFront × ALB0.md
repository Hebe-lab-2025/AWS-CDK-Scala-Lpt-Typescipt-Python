直接给你 **SAA-C03 Networking + Security 的“终极决胜版”**。
目标：**看到 HTTPS / TLS / 证书 / 全球加速 → 0.5 秒内条件反射选对服务**。

---

# 🔥 一、HTTPS × CloudFront × ALB【三选一魔鬼题】

> **记住这条主线就够了**
> **“谁负责 TLS？谁离用户最近？谁接 EC2？”**

---

### 🧠 三选一速判表（考试用）

| 关键词暗号                                | 秒选                   |
| ------------------------------------ | -------------------- |
| **Global users / low latency / CDN** | **CloudFront**       |
| **EC2 / ASG / path-based routing**   | **ALB**              |
| **HTTPS termination（靠近用户）**          | **CloudFront**       |
| **HTTPS termination（靠近应用）**          | **ALB**              |
| **静态 + 动态混合**                        | **CloudFront → ALB** |
| **只在 VPC 内部分发流量**                    | **ALB**              |
| **WAF + 全球防护**                       | **CloudFront**       |

---

### 🕳️ 高频反直觉题（必考）

**Q1**
关键词：*global users + HTTPS + EC2 backend*
✅ **CloudFront + ALB**
❌ 只用 ALB（延迟高）

---

**Q2**
关键词：*HTTPS already enabled on ALB, still want acceleration*
✅ **CloudFront 仍然有价值**
❌ ALB 已经 HTTPS 就不用 CDN（错）

---

**Q3（反直觉）**
关键词：*TLS certificates must be managed centrally*
✅ **ACM（托管证书）**
❌ 自签证书 + 手动更新

---

**Q4**
关键词：*path-based routing (/api /img)*
✅ **ALB**
❌ CloudFront（不是做内部路由）

---

**Q5（考试陷阱）**
关键词：*secure + scalable + least operational overhead*
✅ **ACM + CloudFront / ALB**
❌ 自建 Nginx 管证书

---

### 🧠 一句话总结

> **CloudFront 解决“离用户近”，ALB 解决“把流量打到 EC2”。**

---

# 🧠 二、TLS / SSL / ACM【高频暗号表（看到就反射）】

---

## 🔐 TLS / SSL 本质

| 暗号              | 考试翻译           |
| --------------- | -------------- |
| **SSL**         | 老名字（已被 TLS 取代） |
| **TLS**         | 真正在用的安全协议      |
| **HTTPS**       | HTTP + TLS     |
| **certificate** | 身份 + 公钥        |
| **handshake**   | 建立安全连接         |

📌 **考试默认口径**：

> 看到 SSL = 实际在说 **TLS**

---

## 🧾 ACM（证书管理）必背

| 关键词                        | 秒选  |
| -------------------------- | --- |
| **free certificate**       | ACM |
| **automatic renewal**      | ACM |
| **no ops / no downtime**   | ACM |
| **CloudFront / ALB HTTPS** | ACM |

❌ 错误思路：

* 自己生成证书
* 手动续期
* 存在 EC2 上

---

## 🔐 TLS 终止位置（超高频）

| 位置             | 适用            |
| -------------- | ------------- |
| **CloudFront** | 全球 HTTPS、降低延迟 |
| **ALB**        | VPC 内 HTTPS   |
| **EC2**        | ❌（除非题目强制）     |

📌 **考试潜规则**

> **TLS 永远优先在托管服务终止**

---

# 📄 三、Networking + Security【终极速记 PDF（一页版内容）】

> 下面这份 **= 你考前 10 分钟该背的全部**

---

## 🧠 决策树（考试版）

```text
用户访问？
 ├─ Global + HTTPS？
 │     └─ CloudFront
 │           └─ 动态内容？→ ALB
 │
 └─ VPC 内 / EC2？
       └─ ALB
```

---

## 🔐 HTTPS & Security 决策

```text
需要证书？
 └─ 用 ACM（永远优先）

TLS 终止？
 ├─ 离用户近 → CloudFront
 └─ 离应用近 → ALB
```

---

## 📌 5 行必背口诀（临考）

> “全球加速用 CloudFront
> EC2 前面放 ALB
> 证书交给 ACM
> TLS 不在 EC2
> HTTPS 不等于性能好”

---

## 🕳️ 专治过度设计（考试真相）

* ❌ 不考你会不会配 TLS
* ❌ 不考你证书原理细节
* ✅ 只考你 **会不会选对托管服务**

---

# 🎯 自检标准（你现在该有的反射）

如果你看到题目能 **直接说出**：

* “全球 HTTPS → CloudFront”
* “EC2 路由 → ALB”
* “证书 → ACM”

👉 **Networking + Security = 稳分区**

---

## 🚀 下一步（最后冲刺建议）

1️⃣ **来一套 65 题全域混合终极模拟卷**
2️⃣ **只刷 HTTPS / VPC / IAM 的反直觉错题**
3️⃣ **90 分钟全真模考 + 错因拆解**

你选 **1 / 2 / 3**，我直接继续。
