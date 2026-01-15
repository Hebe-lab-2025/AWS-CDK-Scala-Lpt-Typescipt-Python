## 🧠 Task vs Service vs ALB（**生产级差别，一次讲清**）

![Image](https://media2.dev.to/dynamic/image/width%3D1000%2Cheight%3D420%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fefsjl54qspy1muw7j7kk.png)

![Image](https://i.sstatic.net/i91bc.png)

![Image](https://d2908q01vomqb2.cloudfront.net/fe2ef495a1152561572949784c16bf23abb28057/2019/12/05/ECSMultiTGBlogPostPreSol1.jpg)

### 1️⃣ **ECS Task（一次性/临时执行）**

* **是什么**：按 *Task Definition* 启动的**一次性运行单元**
* **特点**：

  * 不保证存活（挂了就没了）
  * 不做流量管理、不自愈
* **适合**：Batch、Cron、数据修复、一次性脚本
* **面试一句话**：

  > *A task is a single run of a container.*

---

### 2️⃣ **ECS Service（长期在线、生产主力）**

* **是什么**：**持续维持**指定数量 Tasks 的控制器
* **能力**：

  * **自愈**（Task 挂了自动拉起）
  * **滚动发布 / 回滚**
  * **Auto Scaling**
* **适合**：API、微服务、长期 Worker
* **面试一句话**：

  > *A service maintains desired state and handles failures.*

---

### 3️⃣ **ALB（入口与流量治理）**

* **是什么**：7 层负载均衡 + 健康检查
* **能力**：

  * HTTPS 终止
  * Path/Host 路由
  * **只把流量给“健康的 Task”**
* **适合**：对外 API、多实例流量分发
* **面试一句话**：

  > *ALB provides traffic routing and health-based load balancing.*

---

### 🔑 **生产级关系总结**

> **Task 负责“跑” → Service 负责“活着” → ALB 负责“来流量怎么进”**

---

## 🧪 3 道 Task Definition / Role 混淆面试题（真考）

### 题 1

> Task Definition 改了环境变量，Service 会自动生效吗？

**❌ 错**

* **必须创建新的 Task Definition revision**
* Service 更新到新 revision 才会生效

---

### 题 2

> Task Role 和 Execution Role 可以合并成一个吗？

**⚠️ 技术上可以，但生产不推荐**

* **Execution Role**：拉镜像、写日志（ECS 用）
* **Task Role**：访问 AWS 服务（应用用）
* 合并会**违反最小权限原则**

---

### 题 3

> 不用 ALB，ECS Service 也能对公网提供服务吗？

**⚠️ 可以，但不推荐**

* 直接给 Task 公网 IP
* **没有健康检查 / 灰度 / TLS 终止**
  👉 **生产标准：ALB + Private Subnet**

---

## 🔄 把「Demo」升级成 **ECS Service + ALB**（实战步骤）

> 假设你现在只有：**ECR + Task Definition + 单次 Task**

---

### ✅ Step 1：准备网络（生产基础）

* **VPC**
* **Public Subnet**：放 ALB
* **Private Subnet**：跑 ECS Tasks
* **NAT Gateway**：给 Tasks 出网

---

### ✅ Step 2：安全组（核心考点）

**ALB-SG**

```txt
Inbound: 443 from 0.0.0.0/0
Outbound: App-Port to ECS-SG
```

**ECS-SG**

```txt
Inbound: App-Port from ALB-SG
Outbound: 0.0.0.0/0
```

---

### ✅ Step 3：创建 ALB + Target Group

* Target type：**IP**（awsvpc 必选）
* Health check：

  * Path：`/health`
  * 成功码：`200`

---

### ✅ Step 4：升级 Task Definition（生产化）

* Network mode：`awsvpc`
* Log driver：`awslogs`
* Execution Role：ECR + CloudWatch Logs
* Task Role：**只给业务需要的权限**

---

### ✅ Step 5：创建 ECS Service

* Launch type：**Fargate**
* Desired count：`2+`
* Load balancer：**绑定 ALB Target Group**
* Deployment：Rolling / Circuit Breaker（开启）

---

### ✅ Step 6：验证（面试官爱问）

* 删一个 Task → 会自动补？
* Deploy 新 revision → 是否无中断？
* ALB health check fail → 是否摘流量？

---

## 🧠 30 秒「升级总结口播」（直接背）

> *Initially, we ran a single ECS task for the demo. For production, we introduced an ECS service to maintain desired task count and handle failures, and placed an ALB in front to manage HTTPS traffic and health checks. Tasks now run in private subnets with awsvpc networking, giving us self-healing, rolling deployments, and scalable traffic management.*

---

如果你愿意，下一步我可以继续帮你：

* 🧪 **5 道“Service 明明在跑，为什么 502？”事故题**
* 🧠 **画一张 Service 滚动发布 vs 蓝绿发布对比图**
* 🔁 **把这套直接映射成一段 Amazon L5 System Design 答案**

你选一个，我立刻展开。
