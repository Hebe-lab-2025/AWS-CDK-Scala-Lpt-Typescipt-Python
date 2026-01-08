在 **AWS 考试语境里**，**Compute（计算）** 指的是：
👉 **“负责运行代码、处理请求、执行计算逻辑的所有服务”**。

下面是 **SAA / SAP / DVA** 考试中 **Compute 域的完整心智模型**（不是零散背服务名）。

---

## 一、Compute 在 AWS 中“解决什么问题”

一句话总结：

> **Compute = 代码在哪里跑？怎么跑？谁来管？**

---

## 二、AWS Compute 核心服务总览（考试必考）

### 1️⃣ **Amazon EC2** —— 虚拟机（最底层、最灵活）

![Image](https://www.tutorialspoint.com/amazon_web_services/images/architecture.jpg)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/0%2AP9ayxQaaxeBdzcfW.png)

**你负责的：**

* OS（Linux / Windows）
* 补丁
* 扩容方案
* 进程管理

**考试关键词：**

* 长时间运行
* 自定义环境
* 完全控制
* Lift-and-shift

👉 **想要“像自己服务器一样” → EC2**

---

### 2️⃣ **AWS Lambda** —— Serverless（不管服务器）

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AWK_-gPDoCp29u8_MfStF7g.png)

![Image](https://www.supremetech.vn/wp-content/uploads/2025/01/aws-lambda-trigger-event-1.jpg)

**你只负责：**

* 写函数
* 配事件触发

**AWS 负责：**

* 服务器
* 扩缩容
* 高可用

**考试关键词：**

* Event-driven
* 按调用计费
* 自动扩展
* 短任务

👉 **事件触发 / 轻量计算 → Lambda**

---

### 3️⃣ **Amazon ECS** / **Amazon EKS** —— 容器

![Image](https://ec2spotworkshops.com/images/ecs-spot-capacity-providers/amazon_ecs_arch.png)

![Image](https://docs.aws.amazon.com/images/architecture-diagrams/latest/modernize-applications-with-microservices-using-amazon-eks/images/modernize-applications-with-microservices-using-amazon-eks.png)

**共同点：**

* 跑 Docker
* 微服务
* 高并发

**区别（考试常考）：**

| 服务  | 特点                |
| --- | ----------------- |
| ECS | AWS 自家，简单         |
| EKS | 标准 Kubernetes，可移植 |

👉 **已有容器 / 微服务架构 → ECS / EKS**

---

### 4️⃣ **AWS Elastic Beanstalk** —— PaaS（自动化 EC2）

![Image](https://docs.aws.amazon.com/images/elasticbeanstalk/latest/dg/images/aeb-architecture2.png)

![Image](https://blog.shikisoft.com/images/post_imgs/20181008/eb-blue-green-success.jpg)

**你做的：**

* 上传代码

**AWS 自动：**

* EC2
* ALB
* Auto Scaling
* 部署

**考试关键词：**

* 快速部署
* 最少运维
* Web 应用

👉 **“我不想管基础设施” → Beanstalk**

---

### 5️⃣ **Amazon Lightsail** —— 新手/小项目

![Image](https://media.amazonwebservices.com/blog/2016/ls_main_2.png)

![Image](https://cdn.prod.website-files.com/655bc1860a87f22da98dd83c/67585005117c6aa30b1a24f6_67584e7e2dcf9e3a6047dae4_WP4_1.png)

**特点：**

* 一口价
* EC2 + 存储 + 网络打包

👉 **小项目 / 个人站点（考试很少）**

---

## 三、Compute 决策树（考试秒选）

```
要不要管服务器？
├── 不要 → Lambda
├── 要
│   ├── 容器？ → ECS / EKS
│   ├── 快速部署 Web？ → Beanstalk
│   └── 完全控制？ → EC2
```

---

## 四、Compute 常见“考试陷阱”

❌ **Lambda 用来跑 24 小时任务**
→ 超时、错

❌ **用 EC2 处理突发事件流**
→ Lambda 更便宜

❌ **ECS/EKS = 自动免运维**
→ 仍要管容器/集群

---

## 五、一句话记忆版（考前速记）

> **EC2 = 服务器**
> **Lambda = 函数**
> **ECS/EKS = 容器**
> **Beanstalk = 自动化平台**

---

如果你愿意，我可以继续帮你：

* 🔹 把 **Compute vs Storage vs Database** 做成一张 **考试对照表**
* 🔹 出 **10 道 Compute 场景题（秒选型）**
* 🔹 画一张 **“Compute + ALB + Auto Scaling” ASCII 架构图**
* 🔹 用 **中文 + 英文关键词** 帮你做 **SAA-C03 速记卡**

你想下一步往 **考试刷题** 还是 **架构理解**？


