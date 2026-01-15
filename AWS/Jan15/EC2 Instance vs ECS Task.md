## 🧠 EC2 **Instance** vs ECS **Task**（面试高频对比）

| 维度    | **EC2 Instance**           | **ECS Task（Fargate/EC2）**       |
| ----- | -------------------------- | ------------------------------- |
| 本质    | 一台 **虚拟服务器**               | 一次 **容器运行实例**                   |
| 生命周期  | 长期存在（start/stop/terminate） | **短/中期**（启动→运行→结束）              |
| 你要管什么 | OS、补丁、磁盘、网络、进程             | **只管容器配置**（CPU/内存/镜像）           |
| 资源绑定  | 实例级（vCPU/内存固定）             | **Task 级**（每个 task 独立配额）        |
| 扩缩方式  | ASG 扩实例                    | **Service Auto Scaling 扩 task** |
| 部署粒度  | 实例级部署                      | **Task Definition 版本化**         |
| 失败恢复  | 实例挂了要换实例                   | **Task 挂了自动重启**                 |
| 成本模型  | 实例开着就付费                    | **按 task 运行时付费**（Fargate）       |
| 典型场景  | 需要完全控制、遗留系统                | **微服务 / API / Worker**          |

### ✅ 面试一句话

> **Instance 是“机器”，Task 是“一次容器工作单元”。**

---

## 🧪 5 道 Instance / AMI / EBS 易混淆题（真考）

### 1️⃣ **Stop EC2 后，数据会不会丢？**

* **EBS Root 卷**：不丢（默认保留）
* **Instance Store**：**会丢**
  👉 面试点：**存储类型决定数据是否持久**

---

### 2️⃣ **Terminate EC2 后，EBS 一定会删吗？**

* 不一定
* 取决于 **DeleteOnTermination**
  👉 Root 卷默认删，数据卷通常不删（可配置）

---

### 3️⃣ **AMI = 什么？会自动更新吗？**

* **AMI = 实例模板快照**（OS + 软件 + 配置）
* ❌ **不会自动更新**
  👉 新补丁 → 你要 **重新打 AMI**

---

### 4️⃣ **EBS 可以同时挂给多个 EC2 吗？**

* 默认 **不行**
* **Multi-Attach**：只支持特定类型 + 特定文件系统
  👉 面试官在考你“共享存储的坑”

---

### 5️⃣ **Auto Scaling 扩容时，AMI 起什么作用？**

* ASG **用 AMI 创建新实例**
* AMI 决定：OS、启动速度、安全基线
  👉 AMI 老 = 扩出来的实例也老

---

## 🔄 一句话把 **Instance / Container / Function** 串起来（必背）

> **Instance 是“服务器”，Container 是“在服务器上跑的应用进程”，Function 是“只为一次请求存在的代码执行”。**

### 进阶版本（面试加分）

> **抽象层级从低到高：Instance → Container → Function，
> 运维责任逐步下降，弹性与自动化逐步上升。**

---

如果你愿意，下一步我可以帮你做三选一👇

* 🧠 **画一张 Instance → Task → Lambda 的“责任转移图”**（白板友好）
* 🧪 **再出 10 道“选 AMI 还是 Dockerfile？”陷阱题**
* 🔄 **把这套直接映射到一个真实架构（API + Worker）**
