## 🧠 30 秒面试口播：从 Lambda Demo → ECS 生产（可直接背）

> *“I usually start with Lambda to validate the API and business logic fast, because it’s zero-ops and scales instantly for a demo. Once traffic becomes steady or the workload needs longer runtimes, predictable latency, custom dependencies, or background processing, I migrate to ECS—often Fargate—so I can run long-lived containers with controlled CPU/memory, proper health checks, rolling deployments, and autoscaling. If later we need full OS control, special networking, or very stateful workloads, that’s when I consider EC2. In short: Lambda to prove value fast, ECS to run reliably in production, EC2 only when we truly need deep control.”*

---

## 🧪 ECS / Fargate 真·生产踩坑题（带“正确做法”）

### 1) “Service 明明没挂，但用户一直 5xx/超时”

**坑点：** Task 还在跑，但 **ALB health check 失败**（路径/端口/timeout/返回码不匹配）
**正确做法：**

* 对齐 health check：`path / port / healthy threshold / timeout`
* 应用要保证健康检查快速返回（别依赖 DB/第三方）

---

### 2) “Task 起不来：一直 PENDING / STOPPED”

**坑点：**

* Fargate 资源配额/CPU-memory 组合不合法
* 子网 IP 不够（awsvpc 每个 task 一个 ENI）
  **正确做法：**
* 确认 CPU/Memory 组合合法（Fargate 有固定档位）
* 监控子网可用 IP，必要时扩大 CIDR 或多子网分散

---

### 3) “拉镜像失败：CannotPullContainerError / 403”

**坑点：** Execution Role 缺权限，或 **ECR / region** 配错
**正确做法：**

* Execution Role 至少包含 ECR 拉取、日志权限
* registry/account/region 完全一致
* 确认已登录/已推送正确 tag

---

### 4) “容器内能跑，但访问外网失败（例如调用第三方 API）”

**坑点：** Private Subnet 没有 NAT Route，或安全组/网络 ACL 限制出站
**正确做法：**

* Private Subnet → Route Table 指向 NAT Gateway
* SG 出站允许需要的目标（通常先放开，再逐步收紧）

---

### 5) “日志去哪了？CloudWatch 看不到”

**坑点：** log driver 没配、log group 不存在、Execution Role 不允许写日志
**正确做法：**

* Task Definition 配 `awslogs`
* 预建 log group（或允许自动创建）
* Execution Role 加 CloudWatch Logs 权限

---

### 6) “CPU/Memory 明明很低，但延迟很高”

**坑点：**

* 连接池不足/线程模型不匹配
* 单 task 并发能力低（Node/Java 默认配置）
  **正确做法：**
* 调整应用层并发（线程池、连接池、keep-alive）
* 用 target tracking scaling（如 ALB request count / latency）

---

### 7) “部署一升级就掉流量/抖动”

**坑点：** 滚动发布参数不当：`minimumHealthyPercent`、`maximumPercent`、grace period
**正确做法：**

* 开启 deployment circuit breaker（失败自动回滚）
* 配置 health check grace period
* 合理设置 min/max healthy percent

---

## 🔁 Lambda vs ECS vs EC2：什么时候该换（判断表）

| 触发信号（你看到的现象）                      | 该从哪换到哪                   | 面试理由关键词                      |
| --------------------------------- | ------------------------ | ---------------------------- |
| 只是 Demo/MVP、事件驱动、流量不稳定            | **先用 Lambda**            | zero-ops, fast iteration     |
| 需要 **>15 分钟**、长任务/后台 worker       | **Lambda → ECS**         | long-running workload        |
| 冷启动影响明显（尖峰 P95 抖动）                | **Lambda → ECS**         | predictable latency          |
| 依赖复杂（native libs、特定 runtime、持续连接） | **Lambda → ECS**         | custom runtime, dependencies |
| 需要蓝绿/滚动、健康检查、稳定服务形态               | **Lambda → ECS**         | controlled deployments       |
| 需要 GPU/特殊内核模块/深度 OS 调优            | **ECS → EC2**（或 EKS/EC2） | full OS control              |
| 单机状态强（本地磁盘、遗留软件）                  | **容器不合适 → EC2**          | stateful legacy              |
| 团队要 K8s 生态（CRD/Operator/多云）       | **ECS → EKS**            | k8s ecosystem                |

### 一句话背诵版

* **Lambda**：验证价值 + 事件驱动
* **ECS**：生产稳定 + 可控运行时
* **EC2**：极致控制 + 特殊需求

---

如果你把你 Lab 的 **组件清单**（比如：ECR repo、Task Definition 配置、是否 ALB、子网公私、是否 NAT、日志方式）贴 5 行出来，我可以把上面这些“踩坑题”**精准对齐到你这套 Lab**，并给你一份“面试官问到这一步你怎么答”的逐层话术。
