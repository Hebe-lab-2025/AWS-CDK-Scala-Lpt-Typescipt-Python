## 🌍 Amazon CloudFront（CDN）是什么？

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2024/05/15/fig1-comfyui-stable-diffusion-1024x580.png)

![Image](https://docs.aws.amazon.com/images/AmazonCloudFront/latest/DeveloperGuide/images/regional-edge-caches.png)

![Image](https://lucvandonkersgoed.com/wp-content/uploads/2023/09/regional-edge-caches.png?w=1024)

**Amazon CloudFront** 是 AWS 提供的 **内容分发网络（CDN）服务**，通过全球 **Edge Locations（PoP）** 把内容缓存到**离用户最近的地方**，从而 **降低延迟、提升性能、增强安全性**。

---

## 一句话理解

👉 **CloudFront = 把内容提前放到全球“边缘节点”，用户就近访问。**

---

## CloudFront 的核心能力

### 1️⃣ 全球加速（CDN 本质）

* 缓存并分发：

  * 静态内容（图片、视频、JS、CSS、HTML）
  * 动态内容 / API（可配置）
* 用户命中 **Edge 缓存** 时，不回 Region

📌 关键词：*low latency / global users*

---

### 2️⃣ Edge Locations + Regional Edge Cache

* **Edge Location（PoP）**：离用户最近的小缓存
* **Regional Edge Cache**：PoP 与 Region 之间的大缓存
* 逐级命中，**尽量不回源**

```
用户 → Edge（命中？）
        ↓ 否
     Regional Edge Cache（命中？）
        ↓ 否
     Region（S3 / ALB / EC2）
```

---

### 3️⃣ 多种源站（Origin）

CloudFront 可以从以下位置取内容：

* **Amazon S3**（最常见）
* **Elastic Load Balancing**
* **Amazon EC2**
* 任意 HTTP 服务器（含本地机房）

---

### 4️⃣ 安全防护（边缘拦截）

* 集成 **AWS WAF**（防 SQLi/XSS）
* 集成 **AWS Shield**（DDoS）
* 源站 IP 隐藏在 CloudFront 之后

---

### 5️⃣ HTTPS 与证书

* 支持 HTTPS/TLS
* 结合 **AWS Certificate Manager**
* 在 **Edge 层终止 TLS**，性能更好

---

## CloudFront vs S3（常考）

| 项目 | CloudFront | S3  |
| -- | ---------- | --- |
| 角色 | 分发/加速      | 存储  |
| 缓存 | ✅          | ❌   |
| 覆盖 | 全球 Edge    | 区域  |
| 延迟 | 低          | 相对高 |

> **记忆法：S3 存，CloudFront 送**

---

## 典型使用场景

* 🌍 全球网站/APP 加速
* 🎬 视频点播、下载分发
* 🖼️ 静态资源加速
* 🛡️ Web 防护前置
* 🚀 API 加速

---

## 考试速记 🧠

* **Global users + low latency → CloudFront**
* **Static content → CloudFront**
* **Protect ALB/EC2 → CloudFront + WAF**
* **Use Edge Locations → CloudFront**

需要的话，我可以给你做一张 **CloudFront 一页速记卡（Markdown/PDF）** 或出 **5 道高频陷阱题**。
