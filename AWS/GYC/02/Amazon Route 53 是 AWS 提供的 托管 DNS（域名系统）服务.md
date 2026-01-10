## 🌐 Amazon Route 53 是什么？

![Image](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2019/06/05/v2-fig-01.jpg)

![Image](https://jayendrapatil.com/wp-content/uploads/2016/06/Route-53-Complex-Routing-Policies.png)

![Image](https://d1tcczg8b21j1t.cloudfront.net/strapi-assets/32_Route_53_health_checks_12_317621ea21.png)

**Amazon Route 53** 是 AWS 提供的 **托管 DNS（域名系统）服务**，负责把**域名解析成 IP 地址**，并且支持**高可用、低延迟、智能流量路由**。

---

## 一句话理解

> **Route 53 = 互联网“电话簿 + 智能调度员”**
> 把域名翻译成 IP，并把用户带到**最快/最健康**的后端。

---

## Route 53 能做什么？

### 1️⃣ 域名解析（DNS）

* 把 `www.example.com` → IP 地址
* DNS 查询通过 **Edge Locations** 就近完成 → 更快更稳

📌 关键词（考试常见）：

* *DNS*
* *domain name*
* *name resolution*

---

### 2️⃣ 多种智能路由策略（核心卖点）

![Image](https://kodekloud.com/kk-media/image/upload/v1752860904/notes-assets/images/AWS-Certified-SysOps-Administrator-Associate-Route-53-Routing-Policies/latency-routing-policy-route53.jpg)

![Image](https://media.amazonwebservices.com/blog/weighted_then_geo_1.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2Aq_SG4PMBN_LTTHPdOvcphg.png)

**常用路由策略：**

| 策略                | 作用      | 典型场景       |
| ----------------- | ------- | ---------- |
| **Simple**        | 单一记录    | 基础解析       |
| **Weighted**      | 按比例分流   | 灰度发布 / A/B |
| **Latency-based** | 就近访问    | 全球用户       |
| **Failover**      | 主备切换    | 高可用        |
| **Geolocation**   | 按国家/地区  | 合规 / 本地化   |
| **Geoproximity**  | 按距离     | 精细流量控制     |
| **Multi-value**   | 多 IP 返回 | 简单 HA      |

📌 **记忆法**：

> 要“快” → Latency
> 要“稳” → Failover
> 要“控流” → Weighted

---

### 3️⃣ 健康检查（Health Check）

* 定期检查后端（EC2 / ALB / IP）
* 不健康 → **自动停止返回该记录**
* 常与 **Failover Routing** 搭配

---

### 4️⃣ 高可用 & 可扩展

* AWS 托管
* 高并发 DNS 查询
* SLA 级别服务

---

## Route 53 和 CloudFront 的关系（常考）

| 对比     | Route 53       | CloudFront     |
| ------ | -------------- | -------------- |
| 作用     | DNS 解析 & 路由    | 内容加速（CDN）      |
| 是否缓存内容 | ❌              | ✅              |
| 运行位置   | Edge Locations | Edge Locations |
| 典型用法   | 域名 → 资源        | 加速网站           |

👉 **常见组合**：
**Route 53（DNS） → CloudFront（CDN） → ALB / S3**

---

## 一个完整访问流程（帮助理解）

```
用户访问 www.example.com
   ↓
Route 53（DNS 解析，就近 Edge）
   ↓
CloudFront（Edge 缓存）
   ↓（未命中）
ALB / EC2 / S3（Region）
```

---

## 考试 & 面试速记 🧠

* **DNS → Route 53**
* **Global users + lowest latency → Route 53 Latency Routing**
* **Disaster recovery → Route 53 Failover**
* **A/B testing → Route 53 Weighted Routing**

📌 常见陷阱：

* Route 53 **不存内容**
* Route 53 **不是 Load Balancer**

---

如果你愿意，我可以继续帮你：

* 🧠 画一张 **Route 53 路由策略对照速记表**
* 🧪 出 **5 道 Route 53 高频考试陷阱题**
* 📄 做一页 **Route 53 + CloudFront 联合架构速记卡**
