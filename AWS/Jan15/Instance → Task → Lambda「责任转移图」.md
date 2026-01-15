## 🧠 Instance → Task → Lambda「责任转移图」（白板友好）

> 记忆法：**从“我负责一切” → “我只负责应用” → “我只负责函数逻辑”**

```text
            ┌─────────────────────────────────────────────────────────┐
            │                 Responsibility Ownership                │
            │  (Who is on the hook when things break at 2am?)         │
            └─────────────────────────────────────────────────────────┘

EC2 Instance (VM)                ECS Task (Fargate)                     Lambda
┌─────────────────────┐         ┌─────────────────────┐               ┌─────────────────────┐
│ You own:             │         │ You own:             │               │ You own:             │
│ - OS patching        │  SHIFT  │ - Container image    │     SHIFT     │ - Function code      │
│ - SSH access         │ ======> │ - App deps inside    │   ======>     │ - Dependencies       │
│ - Docker runtime     │         │ - CPU/Mem requests   │               │ - Timeout/memory     │
│ - Instance scaling   │         │ - Env vars/secrets   │               │ - Event/HTTP handling│
│ - Disk mgmt (EBS)    │         │ - Task/IAM role      │               │ - IAM role           │
│ - Network hardening  │         │ - Service autoscale  │               │ - Concurrency limits │
│ - Monitoring agents  │         │ - Logs/metrics config│               │ - Logs/metrics config│
└─────────────────────┘         └─────────────────────┘               └─────────────────────┘

AWS owns more (to the right):
EC2: HW + hypervisor
ECS/Fargate: + OS + runtime + host scaling
Lambda: + OS + runtime + scaling + infra completely
```

**一句话口播：**

> *“Moving from EC2 to Fargate shifts OS and host management to AWS, and moving to Lambda shifts even container/runtime concerns so we only own function logic and configuration.”*

---

## 🧪 10 道“选 AMI 还是 Dockerfile？”陷阱题（含标准答案）

> 规则：**要不要“环境可复现 + 可移植”？要就 Dockerfile；要“深度定制 OS/内核/代理”？就 AMI。**

### 1) 你要快速水平扩容 100 个副本，并保证环境一致

* ✅ **Dockerfile**（镜像可复现、版本化，扩容一致）
* ❌ AMI（更像机器快照，版本治理更难）

### 2) 你的应用依赖自定义内核模块/特殊驱动（非 GPU 也可能）

* ✅ **AMI**
* ❌ Dockerfile（容器不适合搞内核级定制）

### 3) 你要在 ECS/Fargate 上跑服务

* ✅ **Dockerfile**（Fargate 需要容器镜像）
* ❌ AMI（那是 EC2 模式的思路）

### 4) 你要在 EC2 上跑，但团队经常“手动改机器导致漂移”

* ✅ **Dockerfile + CI 构建镜像**（或者至少用 Packer 统一构建）
* ❌ 手工 AMI/手工改实例（drift 地狱）

### 5) 你要做蓝绿发布/回滚，且可审计“这版到底部署了啥”

* ✅ **Dockerfile（tag/sha）**
* ❌ AMI（也能做，但可追溯通常更麻烦）

### 6) 你的系统是传统单体，安装步骤复杂、依赖老旧、迁移容器成本高

* ✅ **AMI（短期落地更快）**
* 但长期建议：逐步容器化（拆服务、提炼依赖）

### 7) 你需要把同一份应用同时跑在本地、CI、测试、生产

* ✅ **Dockerfile**（“build once, run anywhere”）
* ❌ AMI（本地/CI 不好复刻）

### 8) 你必须预装安全代理/EDR/公司强制的监控 agent，并且 agent 需要 root/系统级安装

* ✅ **AMI**（系统级更顺）
* Dockerfile 也能装一些 agent，但常受限且不优雅（尤其在 Fargate/Lambda）

### 9) 你的工作负载需要 GPU（深度学习推理）且依赖特定 CUDA/驱动版本

* ✅ **AMI（配好驱动） + 容器镜像（装用户态库）**：现实里经常两者结合
* 纯 Dockerfile 不够（宿主驱动问题）

### 10) 你要做“不可变基础设施”（immutable infra），减少手工变更

* ✅ **Dockerfile（配合镜像发布）** 或 **AMI（Packer 构建）**
* ❌ 直接 ssh 改机器（最危险）

---

## 🔄 映射到真实架构：API + Worker（生产常见）

下面给你一套**能讲清 trade-off 的真实架构**：API 接请求，Worker 异步处理（生成文档/图片/批处理都适用）。

### 方案 A（AWS 原生、成本友好）：**API 用 ECS/Fargate，Worker 用 ECS/Lambda**

```text
Client
  |
  | HTTPS
  v
ALB
  |
  v
ECS Service (Fargate)  ----writes job---->  SQS Queue  ----triggers---->  Worker
(API containers)                               |                           |
                                               |                           |
                                               |                    Option 1: Lambda
                                               |                    - bursty/short jobs
                                               |                    Option 2: ECS Service
                                               |                    - long-running/CPU heavy
                                               v
                                        DynamoDB / RDS (job status)
                                               |
                                               v
                                         S3 (artifacts)
```

**怎么把“AMI vs Dockerfile”塞进这个架构：**

* API（ECS/Fargate）：**Dockerfile**（最合理）
* Worker：

  * 轻量、突发：**Lambda（不需要 Dockerfile 也行，但也支持容器镜像）**
  * 重计算/长任务：**ECS/Fargate + Dockerfile**
* 只有当你不得不跑在 EC2（特殊依赖/系统级 agent）时，才考虑 **AMI**

### 面试口播模板（直接用）

> *“For the API layer, we used ECS Fargate with Docker images for consistent deployments and easy rollback. For async workloads, we decoupled with SQS; bursty short jobs run on Lambda, while long-running or CPU-heavy jobs run as ECS workers. We prefer Dockerfiles for reproducibility, and only use AMIs when we need OS-level customization or legacy constraints.”*

---

如果你想更“白板面试化”，我也可以把上面这套再压成 **30 秒讲解版**（含关键关键词：**at scale / granular control / least privilege / blast radius**）。
