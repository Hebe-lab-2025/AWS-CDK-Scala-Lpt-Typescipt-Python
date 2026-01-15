## 🧠 EC2 vs ECS vs Lambda 决策表（面试/真考版）

| 维度            | **EC2 (自管服务器)**       | **ECS on EC2 (容器 + 自管节点)**          | **ECS Fargate (容器 + 免节点)**     | **Lambda (函数)**              |
| ------------- | --------------------- | ----------------------------------- | ------------------------------ | ---------------------------- |
| 最适合           | 需要“完全控制”的长期服务         | 需要容器编排，但愿意管节点                       | 需要容器编排，但不想管节点                  | 事件驱动/短任务/突发流量                |
| 运维成本          | **最高**（OS、补丁、扩容、AMIs） | 高（还要管 EC2 集群容量）                     | **低**（只管 task）                 | **最低**（只管代码）                 |
| 弹性扩缩          | 手动/ASG                | Service Auto Scaling + EC2 capacity | **Service Auto Scaling + 无节点** | **自动**（并发扩展）                 |
| 启动/冷启动        | 快（常驻）                 | 快（容器常驻/拉镜像）                         | 中（拉镜像+启动 task）                 | 可能冷启动（取决于运行时/VPC）            |
| 运行时限制         | 基本无限制                 | 基本无限制                               | 有限制（但多数够用）                     | 有限制（超时、内存、并发、包大小等）           |
| 长连接/WebSocket | 很适合                   | 适合                                  | 适合                             | 不适合（一般要 API GW/WebSocket 服务） |
| 批处理/任务        | 适合                    | 很适合                                 | **很适合**                        | 适合短任务；大任务要拆分                 |
| 成本模型          | 按实例计费（空闲也付）           | EC2+容器（空闲也付）                        | **按 task vCPU/内存计费**           | 按调用/运行时计费（空闲≈0）              |
| 可观测性          | 自己装/自己管               | 容器 + 节点两层                           | **容器层**（更简单）                   | 函数级（简单但需关联追踪）                |
| 安全隔离          | 你负责                   | 你负责                                 | 更强隔离（task 级）                   | 强隔离（函数级）                     |
| 典型面试一句话       | “我需要最大控制权”            | “我想容器化但能管集群”                        | **“我想容器化且不管服务器”**              | “事件驱动、自动扩缩、按量付费”             |

### ✅ 口头决策口诀（面试好用）

* **要控制/要定制内核/要特定驱动** → EC2
* **要容器 + 不想管节点** → **ECS Fargate**
* **事件触发、短任务、突发流量、尽量少运维** → Lambda
* **有大量稳定常驻服务 + 成本极致优化 + 愿意管节点** → ECS on EC2

---

## 🔁 把架构升级成 ECS Fargate（不需要 EC2）——最小可落地版本

### 目标架构（你现在能直接画在白板上）

* **ALB**（公网入口）
* **ECS Fargate Service**（跑你的容器 API）
* **ECR**（镜像仓库）
* **CloudWatch Logs + Metrics**（日志与监控）
* **(可选) RDS/DynamoDB + SQS/SNS**（数据与异步）

### 迁移步骤（面试回答像“实战做过”）

1. **把应用容器化**：Dockerfile + healthcheck
2. **推镜像到 ECR**：tag + push
3. **建 ECS Cluster（Fargate）**
4. **写 Task Definition**：CPU/Memory、端口、env/secret、日志驱动
5. **建 Service**：desired count、auto scaling、部署策略（rolling）
6. **接入 ALB**：target group health check / path-based routing
7. **网络**：私网 subnets + NAT（出网拉依赖/访问外部）或公有 subnets（简化但不推荐）
8. **权限**：Task Role（访问 DynamoDB/S3/SQS），Execution Role（拉镜像/打日志）
9. **观测**：结构化日志、指标、告警、X-Ray/OTel（可选）

### 常见坑（真考点）

* health check 路径/返回码不一致 → task 不断重启
* Security Group：ALB→Task 的入站没放通
* Task 在私网没 NAT → 拉镜像/访问公网失败
* IAM role 混淆：**TaskRole vs ExecutionRole**
* 镜像太大 → 启动慢（cold deploy 慢）

---

## 🧪 10 道“这套架构哪里可以优化？”面试追问（带参考答题方向）

> 你可以把它当成“系统设计 follow-up 模板”，面试官问啥你都能接。

1. **流量突增时，你的瓶颈在哪？怎么证据化？**

   * 指标：ALB target 5xx、p95 latency、CPU/Memory、队列积压；先定位再扩容（Service Auto Scaling/读写拆分/缓存）

2. **如何做零停机部署？**

   * Rolling + health check；更强：Blue/Green（CodeDeploy），并用 canary/weighted target group

3. **为什么你选择 Fargate 而不是 Lambda？**

   * 需要长连接/稳定常驻/自定义依赖/CPU 密集；Lambda 适合事件短任务

4. **如何降低 Fargate 成本？**

   * 设定合理 CPU/Memory、水平扩容阈值、按需/Spot（Fargate Spot）、压缩镜像、减少空闲副本、请求合并/缓存

5. **数据库成为瓶颈怎么办？**

   * 读写分离、连接池、索引、缓存（Redis）、异步化（SQS）、幂等、限流与降级

6. **如何让系统更安全？**

   * Task 私网、最小权限 IAM、Secrets Manager、WAF、TLS、出站控制、镜像扫描、只暴露 ALB

7. **如何提升可观测性（快速定位一次 500）？**

   * trace id、结构化日志、分布式追踪（X-Ray/OTel）、SLO/告警、关联 ALB access log

8. **如果某个下游 API 慢/挂了，你如何防止雪崩？**

   * 超时、重试（带退避）、熔断、bulkhead、队列缓冲、快速失败 + 降级

9. **如何处理重复请求（幂等）？**

   * 幂等键、去重表/Redis set、SQS FIFO（如果需要严格顺序）、写入条件表达式（DynamoDB）

10. **单区域故障怎么办？**

* Multi-AZ（默认），更高：多区域、全局流量路由（Route53）、数据复制策略（RDS/Aurora global、DynamoDB global tables）

---

## 🚀 你下一步想偏实战：我建议的“最值练”路线（不问你问题，直接给方案）

### 路线 A（最贴近面试 + 最快产出）：**“EC2 → Fargate 迁移小项目”**

* 做一个简单 API（/health, /items）
* ECR 推镜像 → ECS Fargate + ALB
* 加：Auto Scaling + CloudWatch Alarm
* 再加一个 SQS 异步 worker（同样 Fargate）
* 你就能在面试里讲：迁移、扩缩容、可观测性、解耦、成本

### 路线 B（更像真实公司）：**“蓝绿部署 + 回滚演练”**

* CodeDeploy Blue/Green
* 故意发一个会 500 的版本 → 自动回滚
* 这套是非常加分的“Deliver Results + Operational Excellence”

你要是选 **路线 A**，我可以直接把“任务清单 + 画图版架构 + 你面试用的一段 60 秒讲稿（Why Fargate & tradeoff）”一次给你。
