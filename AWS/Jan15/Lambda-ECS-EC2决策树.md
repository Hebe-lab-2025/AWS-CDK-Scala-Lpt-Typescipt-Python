## 🧠 一页「Lambda / ECS / EC2」**决策树（面试白板版）**

![Image](https://d2908q01vomqb2.cloudfront.net/8effee409c625e1a2d8f5033631840e6ce1dcb64/2021/04/09/Decision-Tree.png)

![Image](https://www.whiteboxsolution.com/wp-content/uploads/2022/07/Picture3.png)

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2020/08/21/Figure-1-Architecture-Diagram-1260x606.png)

```
❓ 这是事件驱动 / 请求驱动？
 ├─ 是（HTTP / Queue / Event）
 │   └─ 执行 ≤ 15 分钟？
 │       ├─ 是 → ✅ Lambda
 │       └─ 否 → ❌ Lambda 不行 → ECS / EC2
 │
 └─ 否（常驻服务 / 长连接 / 自管进程）
     └─ 需要你管理 OS / 特殊依赖 / GPU？
         ├─ 是 → ✅ EC2
         └─ 否 → ECS
             └─ 不想管 EC2？
                 ├─ 是 → ECS Fargate
                 └─ 否 → ECS on EC2
```

### 🧠 速记口诀（面试 3 秒）

* **短、突发、事件** → **Lambda**
* **长跑、容器化、可控** → **ECS**
* **重、定制、底层控制** → **EC2**

---

## 🧪 10 道「给场景选计算服务」真题（含一句话答案）

1. **API 请求 → 轻量计算 → 峰值不确定**
   👉 **Lambda**（自动扩缩、无服务器管理）

2. **图片/视频转码，10–30 分钟**
   👉 **ECS**（超 15 分钟，容器更稳）

3. **WebSocket / 长连接服务**
   👉 **ECS / EC2**（Lambda 不适合长连接）

4. **夜间批处理，固定时间跑 2 小时**
   👉 **ECS Task（EventBridge 触发）**

5. **需要 GPU / CUDA**
   👉 **EC2**（最大控制权）

6. **消息队列消费者，吞吐高、可并发**
   👉 **ECS Worker**（并发、可控、好限流）

7. **Webhook 接收 + 简单校验**
   👉 **Lambda**（最省事）

8. **遗留 Java 应用，依赖 OS 配置**
   👉 **EC2**（避免容器化成本）

9. **CPU/内存稳定、7x24 API 服务**
   👉 **ECS Service**（可扩缩、比 EC2 省运维）

10. **偶发运维脚本 / glue code**
    👉 **Lambda**（成本最低）

---

## 🔁 把你的 Lab 升级成「API（Service）+ Worker（Task）」**面试级架构故事**

![Image](https://docs.aws.amazon.com/images/whitepapers/latest/next-generation-oss/images/service-orchestration-architecture.png)

![Image](https://miro.medium.com/1%2AomoHcztIcGz5MdLjx-1ePA.jpeg)

![Image](https://codefresh.io/wp-content/uploads/2023/07/AWS-Microservice.png)

### 🎯 架构目标

* **前端请求快**（API 不被重任务拖慢）
* **重活异步化**（Worker 可独立扩缩）
* **失败可重试、可观测**

---

### 🧩 架构拆分（面试要点）

**API（Service）层**

* **API Gateway + Lambda**（或 ECS Service）
* 职责：

  * 参数校验
  * 鉴权 / CORS
  * **快速返回 202 Accepted**
  * 投递消息到队列（SQS）

**Worker（Task）层**

* **ECS Task / Fargate**
* 职责：

  * 执行耗时任务（生成、计算、IO）
  * 可并发扩缩
  * 失败重试 / DLQ

**中间件**

* **SQS**：解耦、削峰
* **CloudWatch**：日志 + 指标
* （可选）**DynamoDB / S3**：状态 / 结果存储

---

### 🧠 为什么这是“正确的计算服务选择”（面试官想听）

* **Lambda 做 API**：

  * 冷启动可接受
  * 自动扩缩，应对突发流量
* **ECS 做 Worker**：

  * 突破 15 分钟限制
  * 控制并发，避免下游被打爆
  * 容器化，依赖清晰
* **SQS 解耦**：

  * API 不阻塞
  * Worker 可水平扩展

---

### 🗣️ 面试版 STAR 故事（可直接背）

**Situation**

> In my lab, the API was directly doing heavy processing, which caused timeouts under load.

**Task**

> I needed to improve reliability and scalability without overcomplicating the system.

**Action**

> I split the system into an API service and a worker.
> The API layer uses Lambda to handle requests and enqueue jobs to SQS.
> The worker layer runs as ECS tasks that process jobs asynchronously and scale based on queue depth.

**Result**

> API latency dropped significantly, the system handled spikes smoothly, and the architecture became easier to scale and reason about.

---

### 🧠 一句话总结（面试收官）

> *“I use Lambda for fast, stateless APIs, ECS for long-running or heavy workers, and EC2 only when I need full OS-level control.”*

---

如果你愿意，我可以下一步直接帮你做三件事之一：

* 🧠 **画一张“API → SQS → ECS Worker”的白板图（带扩缩点）**
* 🧪 **再出 10 道“Lambda vs ECS Fargate vs ECS on EC2”陷阱题**
* 🔧 **把你现有 Lab 的代码/模板改成完整 API + Worker（可 deploy）版本**
