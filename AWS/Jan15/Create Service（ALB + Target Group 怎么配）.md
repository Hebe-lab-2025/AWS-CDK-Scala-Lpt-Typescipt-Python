## 🔄 Create Service（ALB + Target Group 怎么配）

下面按**面试会追问的“为什么”**来讲（不是点点点教程）。

### 1) 先定前提：你用的网络模式

* **Fargate 基本必选 `awsvpc`**
* **EC2 也建议 `awsvpc`（生产更一致）**
* 结论：**Target Group 选 `IP` 类型**（而不是 Instance）

  * `awsvpc` 下每个 Task 有自己的 ENI / IP，所以 TG 应该注册 **Task IP**

---

### 2) ALB Listener：80/443 怎么配？

* **最常见**：

  * `:80` → 重定向到 `:443`
  * `:443` → Forward 到 Target Group
* 面试点：HTTPS 终止在 ALB，后端走 HTTP（也可以端到端 TLS，但复杂度更高）

---

### 3) Target Group：端口、健康检查、路径怎么选？

**关键：TG 的“端口”指的是你容器暴露的应用端口（ContainerPort）**

* **Port**

  * 如果你服务在容器里监听 `8080`，那 TG port 就是 `8080`
  * **不要**写成 80 只是因为 ALB 监听 80（这是最常见坑）

* **Health check**

  * Path：`/health` 或 `/ping`（必须是**不依赖 DB 的轻量**检查，避免“下游抖动导致全员被判死”）
  * Matcher：`200`（或 `200-399` 视业务）
  * Interval/Timeout：典型 `30s / 5s`
  * Healthy threshold：`2-3`；Unhealthy threshold：`2-3`

* **Deregistration delay（优雅下线）**

  * 典型 30–60s：让旧 Task 有时间处理完 in-flight 请求
  * 面试关键词：**graceful shutdown**

---

### 4) ECS Service 里 ALB 绑定怎么填？

Create Service 时一般要填：

* **Load balancer type**：Application Load Balancer
* **Listener**：选 ALB 的 `:443`（或 :80）
* **Target group**：选上面建好的 TG
* **Container name / Container port**

  * Container name：Task Definition 里那一个 container 的名字
  * Container port：你的应用监听端口（例如 8080）

> 面试金句：**“ALB 把流量送到 Target Group；Target Group 注册的是 Task IP:ContainerPort。”**

---

### 5) Service 的最关键参数：Desired / Min/Max / Deployment

* **Desired count**：最少 2（避免单点）
* **Auto Scaling**：按 CPU、RequestCountPerTarget、队列长度等
* **Deployment config**

  * **Minimum healthy percent**（典型 100）
  * **Maximum percent**（典型 200）
    => 滚动发布时“先加新再减旧”，降低中断风险

---

## 🧪 5 道 Task Definition / Task Role / Execution Role 易混题（带答案）

### 题 1：Task 拉不下来 ECR 镜像，是 Task Role 不够吗？

**答案：不是。**
拉镜像用的是 **Execution Role**（ECS agent 需要它去 ECR auth/pull）。

---

### 题 2：容器代码里要读 S3 / 写 DynamoDB，该给哪个 Role？

**答案：Task Role。**
因为这是“应用运行时访问 AWS 资源”的权限。

---

### 题 3：把 S3 权限加到 EC2 Instance Profile，能跑，但为什么不推荐？

**答案：权限边界不对。**
会导致“同一台 EC2 上的其它 Task 也可能蹭到权限”，爆炸半径变大。正确是 **按 Task 授权**。

---

### 题 4：用了 `awslogs`，日志写不进 CloudWatch，是 Task Role 还是 Execution Role？

**答案：Execution Role（通常）。**
写 CloudWatch Logs 多半是 ECS agent/日志驱动在干活；缺 `logs:CreateLogStream/PutLogEvents` 会失败。

---

### 题 5：你把 Secret 放 Secrets Manager，注入成环境变量，是谁去读 Secret？

**答案：通常是 Execution Role。**
因为是“启动容器前注入”，不是你应用代码运行时自己去调 API（后者才用 Task Role）。

---

## 🧠 ECS Service 生命周期图（真考）

```text
[You] Update Service / Deploy
        |
        v
[ECS Service Scheduler]  --(desired count / deployment config)-->
        |
        v
[Place Task] -> pick cluster capacity (Fargate or EC2)
        |
        v
[Pull Image from ECR]  (ExecutionRole)
        |
        v
[Create ENI + IP] (awsvpc) + attach SG
        |
        v
[Start Container] -> run ENTRYPOINT/CMD
        |
        v
[Send logs/metrics] -> CloudWatch (awslogs, metrics)
        |
        v
[Register Target] -> Target Group gets Task IP:Port
        |
        v
[ALB Health Checks]
   | healthy                 | unhealthy
   v                         v
[Serve Traffic]        [Stop Task + Replace]
   |
   v
[Scale Out/In] (Auto Scaling policies)
   |
   v
[Rolling Update]
   - start new tasks
   - wait healthy
   - drain old targets (dereg delay)
   - stop old tasks
```

---

如果你要我再“更真考”，我可以继续加一层：
**面试官给你一个现象（例如：ALB 502 / Target unhealthy / 部署卡住 / 任务一直 Pending）→ 我给你 3 步定位路径 + 最可能的 2 个根因。**
