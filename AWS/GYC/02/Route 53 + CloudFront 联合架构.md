下面给你一整套 **👉 可直接复制到 GitHub 的 Markdown 内容**，完全按 **AWS 考试（SAA / SOA / DVA）取向**设计，覆盖你要的 **3 个部分**：

* 🧠 路由策略对照速记表
* 🧪 5 道高频考试陷阱题
* 📄 Route 53 + CloudFront 联合架构一页速记卡

---

# 🌐 Amazon Route 53 一页速记卡（Exam Ready）

> 目标：**看到“DNS / 路由 / 延迟 / 故障转移 / 流量控制”，立刻选对 Route 53 策略**

---

## 一、🧠 Route 53 路由策略总对照表（核心）

| 路由策略                   | 解决什么问题 | 典型关键词                | 一句话秒记             |
| ---------------------- | ------ | -------------------- | ----------------- |
| **Simple**             | 单一资源   | single endpoint      | **只有一个就选它**       |
| **Weighted**           | 流量比例控制 | A/B test, percentage | **按比例分流**         |
| **Latency-based**      | 就近访问   | lowest latency       | **选延迟最低的 Region** |
| **Failover**           | 高可用    | active/passive, DR   | **主挂了切备用**        |
| **Geolocation**        | 按国家/地区 | country-based        | **按用户位置规则**       |
| **Geoproximity**       | 按距离+权重 | distance-based       | **距离 + 偏移量**      |
| **Multi-Value Answer** | 简单负载均衡 | multiple healthy IPs | **DNS 级 LB**      |

---

## 二、🧠 路由策略“关键词 → 秒选”反射表

| 题目关键词                        | 秒选策略                         |
| ---------------------------- | ---------------------------- |
| **single resource**          | Simple                       |
| **split traffic 90/10**      | Weighted                     |
| **A/B testing**              | Weighted                     |
| **lowest latency**           | Latency-based                |
| **active-passive DR**        | Failover                     |
| **country restriction**      | Geolocation                  |
| **users nearest location**   | Latency-based / Geoproximity |
| **DNS-level load balancing** | Multi-Value Answer           |

---

## 三、🧪 Route 53 高频考试陷阱题（5 道）

---

### ❌ 陷阱题 1

**题目**：

> Route traffic to the closest AWS region.

❌ 错误选项：

* Geolocation routing

✅ 正确答案：
👉 **Latency-based routing**

📌 原因：

* Geolocation = 按国家规则
* Latency = 按网络延迟

---

### ❌ 陷阱题 2

**题目**：

> Route 80% traffic to version A and 20% to version B.

❌ 错误选项：

* Simple routing

✅ 正确答案：
👉 **Weighted routing**

---

### ❌ 陷阱题 3

**题目**：

> Automatically route traffic to a secondary endpoint if the primary fails.

❌ 错误选项：

* Multi-value answer

✅ 正确答案：
👉 **Failover routing**

📌 原因：

* Failover = 健康检查 + 主备切换

---

### ❌ 陷阱题 4

**题目**：

> Route users from Europe to EU servers, US users to US servers.

❌ 错误选项：

* Latency-based routing

✅ 正确答案：
👉 **Geolocation routing**

---

### ❌ 陷阱题 5

**题目**：

> Provide basic DNS-level load balancing with health checks.

❌ 错误选项：

* ALB only

✅ 正确答案：
👉 **Multi-Value Answer routing**

---

## 四、📄 Route 53 + CloudFront 联合架构速记图（ASCII）

```text
                🌍 Global Users
                       |
                       v
                ┌──────────────┐
                │   Route 53   │
                │ (DNS Layer)  │
                │  Routing     │
                └───────┬──────┘
                        |
                        v
              ┌───────────────────┐
              │   CloudFront       │
              │ (Edge Locations)  │
              │  CDN / Cache       │
              └───────┬───────────┘
                      │ Cache Miss
                      v
        ┌────────────────────────────────┐
        │           AWS Region            │
        │                                │
        │   ┌──────────────┐             │
        │   │   ALB / EC2  │             │
        │   └──────────────┘             │
        │          ▲                     │
        │   ┌──────────────┐             │
        │   │      S3      │             │
        │   └──────────────┘             │
        └────────────────────────────────┘
```

📌 **分层记忆**：

* **Route 53** → 决定“去哪里”
* **CloudFront** → 决定“快不快”
* **ALB / S3** → 真正提供内容

---

## 五、Route 53 + CloudFront 考试“反直觉”规则（必背）

```text
❗ Route 53 ≠ Load Balancer（它是 DNS）
❗ CloudFront ≠ Region（它是 Edge）
❗ 延迟问题 → CloudFront / Latency routing
❗ 高可用 → Failover routing + Health Check
❗ A/B 测试 → Weighted routing
```

---

## 六、30 秒考前自检清单

* 问的是 **DNS 决策** 还是 **内容加速**？
* 是 **按比例 / 按延迟 / 按国家**？
* 有没有 **failover / DR / health check**？
* 用户是不是 **全球分布**？

---

## ✅ 一句话终极总结（考试 / 面试通用）

> Amazon Route 53 是 AWS 的全球 DNS 服务，通过多种路由策略实现流量控制和高可用性，并常与 CloudFront 结合，为全球用户提供低延迟、高可用的访问体验。

---

### 👉 下一步我可以继续给你（同样 Markdown 可复制）：

* 🔥 **Route 53 路由策略“终极决策树”**
* 🧪 **10 道 Route 53 + CloudFront 混合陷阱题**
* 🧠 **DNS / CDN / ALB / Global Accelerator 对比速记卡**

你直接选一句：
👉 **“做决策树” / “再来 10 道陷阱题” / “做服务对比卡”**
