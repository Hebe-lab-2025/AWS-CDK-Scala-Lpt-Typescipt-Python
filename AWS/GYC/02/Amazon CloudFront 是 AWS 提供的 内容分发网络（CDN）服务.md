## ☁️ Amazon CloudFront 是什么？

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/05/12/Fig2-2-1024x602.png)

![Image](https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/product-categories/networking/approved/images/Cloudfront-Map.382304d1adf6722096035baea6bb2eba9816642b.png)

![Image](https://www.dacast.com/wp-content/uploads/2021/02/Cloudfront-CDN-Content-Delivery-Network.png)

**Amazon CloudFront** 是 AWS 提供的 **内容分发网络（CDN）服务**，核心目标只有一个：

👉 **把内容缓存到离用户最近的 Edge Location，让访问更快、更稳、更安全。**

---

## 一句话理解

> **CloudFront = 把你的网站内容提前放到全球“就近仓库”，用户从最近的地方拿。**

---

## CloudFront 能做什么？

### 1️⃣ 全球加速（最核心）

* 缓存 **静态内容**：图片、视频、JS、CSS、HTML
* 也可加速 **动态内容 / API**
* 用户请求不必每次回到远端 Region

📌 关键词（考试必杀）：

* *low latency*
* *global users*
* *fast content delivery*

---

### 2️⃣ 使用 Edge Locations（边缘节点）

* CloudFront 运行在 **Edge Locations**
* Edge Location 离用户很近 → 延迟低
* 命中缓存时，**不会访问后端**

---

### 3️⃣ 保护源站（安全）

CloudFront 常作为**第一道防线**：

* 集成 **AWS WAF**（防 SQLi / XSS）
* 集成 **AWS Shield**（防 DDoS）
* 隐藏源站 IP（不直接暴露 EC2 / ALB）

---

### 4️⃣ 支持多种源站（Origin）

CloudFront 可以从哪里取内容？

* **Amazon S3**（最常见）
* **Elastic Load Balancing**
* **Amazon EC2**
* 任意 HTTP Web Server（On-prem 也行）

---

### 5️⃣ HTTPS & 证书

* 支持 HTTPS（TLS）
* 可直接集成 **AWS Certificate Manager**
* 证书在 Edge 层终止（性能好）

---

## CloudFront 访问流程（超重要）

```
用户
 ↓
最近的 Edge Location
 ↓（缓存命中？）
是 → 直接返回（最快）
否 → 回源（S3 / ALB / EC2）
```

👉 **能在 Edge 解决的，就不回 Region**

---

## CloudFront vs S3（考试常混）

| 对比项  | CloudFront | S3  |
| ---- | ---------- | --- |
| 作用   | 加速分发       | 存储  |
| 是否缓存 | ✅          | ❌   |
| 面向用户 | 全球         | 区域  |
| 延迟   | 低          | 相对高 |

📌 **记忆法**：

> **S3 存内容，CloudFront 送内容**

---

## 典型使用场景

* 🌍 全球用户访问网站
* 🎬 视频点播 / 流媒体
* 🖼️ 图片 / 静态资源加速
* 🛡️ 网站防 DDoS
* 🚀 API 加速

---

## 考试 & 面试速记 🧠

* **Global users + low latency → CloudFront**
* **Static content → CloudFront**
* **Protect ALB / EC2 → CloudFront + WAF**
* **Use Edge Locations → CloudFront**

---

如果你愿意，我可以继续帮你：

* 🧠 画一张 **CloudFront + Edge + Region 总架构图**
* 🧪 出 **5 道 CloudFront 高频考试陷阱题**
* 📄 做一页 **CloudFront 速记卡（Markdown / PDF）**
