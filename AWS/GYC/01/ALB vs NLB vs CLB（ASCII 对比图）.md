太好了，这一组正好是 **SAA-C03 必考负载均衡组合拳**。
我按「**图 → 题 → 模板 → 反例**」一次性给你打通 👇

---

## 一、ALB vs NLB vs CLB（ASCII 对比图）

```
                ┌───────────────┐
Users (HTTP) →  │      ALB       │  Layer 7
                │  HTTP / HTTPS  │
                │  path / host   │
                └───────┬───────┘
                        │
                    EC2 / ECS
```

```
                ┌───────────────┐
Users (TCP) →   │      NLB       │  Layer 4
                │  TCP / UDP     │
                │  ultra low lat │
                └───────┬───────┘
                        │
                    EC2 / ECS
```

```
                ┌───────────────┐
Users →         │      CLB       │  (Legacy)
                │  L4 + L7 混合  │
                │  功能有限      │
                └───────┬───────┘
                        │
                       EC2
```

### 一眼秒记表

| 对比项     | ALB          | NLB       | CLB        |
| ------- | ------------ | --------- | ---------- |
| OSI 层   | **L7**       | **L4**    | 混合         |
| 协议      | HTTP / HTTPS | TCP / UDP | HTTP / TCP |
| 路径路由    | ✅            | ❌         | ❌          |
| Host 路由 | ✅            | ❌         | ❌          |
| Web API | ✅ **首选**     | ❌         | ⚠️         |
| 延迟      | 低            | **超低**    | 较高         |
| 考试地位    | ⭐⭐⭐⭐⭐        | ⭐⭐⭐⭐      | ❌ 不推荐      |

---

## 二、10 道 ALB 场景「秒选题」

1️⃣ **REST API + HTTPS**
👉 **ALB**

2️⃣ **/users → 服务A，/orders → 服务B**
👉 **ALB（Path-based routing）**

3️⃣ **api.example.com / admin.example.com**
👉 **ALB（Host-based routing）**

4️⃣ **微服务架构 + HTTP**
👉 **ALB**

5️⃣ **ECS Web Service 暴露给公网**
👉 **ALB**

6️⃣ **需要与 WAF 集成**
👉 **ALB**

7️⃣ **需要基于 URL 做流量分发**
👉 **ALB**

8️⃣ **HTTP header 决策路由**
👉 **ALB**

9️⃣ **WebSocket 应用**
👉 **ALB**

🔟 **需要健康检查（HTTP 状态码）**
👉 **ALB**

> 考试口诀：
> **“只要看到 HTTP / API / Path / Host → 秒选 ALB”**

---

## 三、ALB + ASG + ECS（整套考试模板）

### 🎯 典型题干长这样

> A highly available web application with unpredictable traffic, running containerized services, needs automatic scaling and path-based routing.

### 🧠 秒拆关键词

* web / HTTP → **ALB**
* unpredictable traffic → **Auto Scaling**
* containerized → **ECS**
* high availability → **Multi-AZ**

---

### 🏗️ 标准答案架构（ASCII）

```
          Users
            |
         (HTTPS)
            |
        ┌────────┐
        │  ALB   │  ← L7 routing
        └────────┘
            |
     ┌────────────────┐
     │ ECS Service     │
     │ (Auto Scaling)  │
     └────────────────┘
        |          |
     Task        Task
   (AZ-1)      (AZ-2)
```

### 📌 考试用一句话模板（直接背）

> Use an **Application Load Balancer** to distribute HTTP traffic to an **ECS service** running across multiple Availability Zones, with **Auto Scaling** enabled to handle unpredictable traffic.

---

## 四、反例题：❌ 为什么不能用 NLB 做 Web API？

### ❓ 反例题目

> A team uses a Network Load Balancer to expose a RESTful Web API.
> They need routing based on URL paths such as `/login` and `/orders`.
> The solution does not work as expected. Why?

---

### ❌ 错误原因（考点直击）

| 点              | NLB | Web API 需要 |
| -------------- | --- | ---------- |
| OSI 层          | L4  | **L7**     |
| 是否理解 HTTP      | ❌   | ✅          |
| Path / Host 路由 | ❌   | ✅          |
| Header         | ❌   | ✅          |

👉 **NLB 不理解 HTTP 内容，只看 TCP 包**

---

### ✅ 正确做法（标准答案）

> Replace the Network Load Balancer with an **Application Load Balancer**, which operates at Layer 7 and supports HTTP path-based routing.

---

### 🧠 反例记忆钩子

> **“NLB 看不到 /login”**
> **“ALB 才看得懂 URL”**

---

## 最后给你一张「考试秒选口诀」

```
HTTP / API / Path / Host
        ↓
       ALB

TCP / 超低延迟 / millions RPS
        ↓
       NLB

老系统 / Legacy
        ↓
       CLB（能不用就不用）
```

---

如果你愿意，下一步我可以直接给你：
🔹 **15 道 ALB vs NLB 陷阱题（真题风格）**
🔹 **ELB 全家桶一页速记脑图**
🔹 **SAA-C03 负载均衡决策树（看到题干 3 秒出答案）**

你说 **继续哪个** 👇
