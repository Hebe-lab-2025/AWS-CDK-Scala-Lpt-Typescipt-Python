## ALB 是啥？（一句话先记住）

> **ALB = Application Load Balancer（应用层负载均衡器）**
> 👉 **专门在 HTTP / HTTPS 层，把用户请求分发到后端服务**

---

## 一、ALB 属于哪一类服务？

**ALB 属于 AWS 的 Load Balancing（负载均衡）服务**
全名：**Elastic Load Balancing（ELB）**

ELB 家族一共 3 个（考试必考）：

| 类型      | 工作层         | 典型用途      |
| ------- | ----------- | --------- |
| **ALB** | **L7（应用层）** | Web / API |
| **NLB** | L4（传输层）     | 高性能 TCP   |
| **CLB** | L4 + L7     | 旧服务（基本淘汰） |

---

## 二、ALB 到底“干什么”？

### 核心职责（记这 5 个）

1. **HTTP / HTTPS 请求分发**
2. **基于规则转发（非常重要）**
3. **健康检查**
4. **SSL / TLS 终止**
5. **高可用（跨 AZ）**

![Image](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2019/10/06/illustration-2.png)

![Image](https://www.learnaws.org/assets/img/aws-alb-request-routing/path-based-routing.png)

---

## 三、ALB 最重要的能力（考试高频）

### 1️⃣ 基于规则转发（ALB 的灵魂）

ALB **不是随机转发**，而是**“看请求内容再转”**：

| 规则类型       | 示例                    |
| ---------- | --------------------- |
| Path-based | `/api → ECS`          |
| Host-based | `admin.xxx.com → EC2` |
| Header     | `User-Agent / token`  |

📌 **一句话考点**：

> **“需要根据 URL / Host 决定转发” → ALB**

---

### 2️⃣ ALB 能接什么后端？

| 后端类型               | 是否支持                 |
| ------------------ | -------------------- |
| EC2                | ✅                    |
| Auto Scaling Group | ✅                    |
| ECS                | ✅                    |
| EKS                | ✅                    |
| Lambda             | ✅（唯一能直连 Lambda 的 LB） |

👉 **ALB = Web + Serverless 的桥梁**

---

### 3️⃣ SSL 在哪终止？

* HTTPS 在 **ALB 解密**
* 后端走 HTTP

📌 好处：

* 后端不用管证书
* 性能更好

---

## 四、ALB vs NLB（必考对比）

| 对比点     | ALB          | NLB       |
| ------- | ------------ | --------- |
| OSI 层   | L7           | L4        |
| 协议      | HTTP / HTTPS | TCP / UDP |
| 转发规则    | 内容级          | 端口级       |
| Web API | ✅            | ❌         |
| 延迟      | 低            | **极低**    |
| 固定 IP   | ❌            | ✅         |

📌 **口诀**：

> **看内容 → ALB**
> **看端口 / 要极低延迟 → NLB**

---

## 五、ALB 在考试里的“典型场景”

✅ **微服务架构**

* `/user → service A`
* `/order → service B`

✅ **蓝绿 / 灰度发布**

* 规则控制流量

✅ **EC2 + Auto Scaling Web 应用**

---

## 六、一个最经典的考试架构图（脑中要有）

![Image](https://docs.aws.amazon.com/images/autoscaling/ec2/userguide/images/elb-tutorial-architecture-diagram.png)

![Image](https://media2.dev.to/dynamic/image/width%3D1000%2Cheight%3D500%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fddvi9pya9qo8o8zs6a8o.png)

```
User
 ↓
ALB (HTTPS)
 ↓
Target Group
 ↓
EC2 / ECS / Lambda
```

---

## 七、30 秒口试版回答（面试 / 考试都能用）

> **ALB is a Layer 7 load balancer in AWS that routes HTTP/HTTPS requests based on content such as URL paths or host headers, supports EC2, ECS, EKS, and Lambda as targets, and is commonly used for web applications and microservices.**

---

如果你愿意，我可以下一步帮你：

* 🔹 **画一张 ALB vs NLB vs CLB ASCII 对比图**
* 🔹 **出 10 道 ALB 场景秒选题**
* 🔹 **把 ALB + ASG + ECS 串成一整套考试模板**
* 🔹 **讲一个“为什么不能用 NLB 做 Web API”的反例题**

你想继续 **刷题** 还是 **架构图**？
