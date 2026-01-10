下面用**中文、通俗 + 考试/入门友好**的方式，把 **IaaS / PaaS / SaaS / FaaS** 一次讲清楚 👇

---

## 一、IaaS（基础设施即服务）

![Image](https://webimages.mongodb.com/_com_assets/cms/l1nldf38un1vw8nnz-image6.png?auto=format%252Ccompress)

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2022/02/09/Figure1-IBMMQ-1024x675.png)

![Image](https://uptimerobot.com/knowledge-hub/wp-content/uploads/2025/02/cloud-infrastructure-components.png)

**一句话理解：**
👉 **云厂商给你“虚拟电脑 + 网络 + 硬盘”，你自己装系统、部署应用。**

### 你负责什么？

* 操作系统（Linux / Windows）
* 应用程序
* 运行环境、补丁、配置

### 云厂商负责什么？

* 物理服务器
* 虚拟化
* 网络、机房、电力

### AWS 中的典型 IaaS

* **Amazon EC2**：虚拟服务器
* **Amazon S3**：对象存储
* **Amazon VPC**：私有网络

📌 **适合场景**：
需要**最大控制权**、自定义系统、传统架构迁移（Lift & Shift）

---

## 二、PaaS（平台即服务）

![Image](https://zd-brightspot.s3.us-east-1.amazonaws.com/wp-content/uploads/2021/07/19072414/Paas.png)

![Image](https://docs.aws.amazon.com/images/elasticbeanstalk/latest/dg/images/aeb-architecture2.png)

![Image](https://docs.aws.amazon.com/images/AmazonRDS/latest/UserGuide/images/aws-cloud-deployment-architecture.png)

**一句话理解：**
👉 **你只管写代码，服务器、扩缩容、运维平台帮你搞定。**

### 你负责什么？

* 业务代码
* 应用逻辑

### 云厂商负责什么？

* 操作系统
* 运行环境
* 自动扩缩容
* 部署、监控、补丁

### AWS 中的典型 PaaS

* **AWS Elastic Beanstalk**：一键部署 Web 应用
* **Amazon RDS**：托管数据库

📌 **适合场景**：
想**快速上线应用**，不想管运维细节

---

## 三、SaaS（软件即服务）

![Image](https://citrusbug.com/wp-content/uploads/use-cases-of-saas-application-development.webp)

![Image](https://www.cts-companies.com/wp-content/uploads/2024/06/365.webp)

![Image](https://www.uctoday.com/wp-content/uploads/2017/04/slack-review-team-collaboration-software.jpg)

![Image](https://www.glowbl.com/blog/wp-content/uploads/2024/04/1-19.png)

**一句话理解：**
👉 **直接用软件，连代码都不用写。**

### 你负责什么？

* 注册账号
* 使用功能
* 配置少量设置

### 云厂商负责什么？

* 软件开发
* 运维
* 升级
* 安全
* 扩展性

### 常见 SaaS 例子

* **Microsoft 365**：办公软件
* **Netflix**：视频平台
* **Slack**：团队协作

📌 **适合场景**：
只想**用功能，不关心技术实现**

---

## 四、FaaS（函数即服务 / Serverless）

![Image](https://stackify.com/wp-content/uploads/2017/05/what-is-function-as-a-service-serverless-architectures-are-here-11196.png)

![Image](https://docs.aws.amazon.com/images/lambda/latest/dg/images/event-driven-architectures-figure-7.png)

![Image](https://docs.cloud.google.com/static/workflows/images/workflows-run-visualization.svg)

**一句话理解：**
👉 **只写一个函数，事件触发就运行，用完即停。**

### 你负责什么？

* 函数代码（逻辑）

### 云厂商负责什么？

* 服务器
* 扩容
* 高可用
* 计费
* 运维

### AWS 中的典型 FaaS

* **AWS Lambda**

### 常见触发方式

* HTTP 请求（API）
* 文件上传（S3）
* 数据库变更
* 消息队列

📌 **适合场景**：

* 事件驱动
* 流量不稳定
* 想 **按毫秒计费、省钱**

---

## 五、四种模式一张表对比（超好记）

| 模型       | 你管什么    | 典型例子                  |
| -------- | ------- | --------------------- |
| **IaaS** | 系统 + 应用 | EC2、VPC               |
| **PaaS** | 代码      | Elastic Beanstalk、RDS |
| **FaaS** | 函数      | Lambda                |
| **SaaS** | 什么都不管   | Microsoft 365、Slack   |

---

## 六、考试/面试快速口诀 🧠

* **要控制权 → IaaS**
* **要快上线 → PaaS**
* **只用功能 → SaaS**
* **事件触发 / 不想管服务器 → FaaS**

如果你愿意，我可以下一步帮你做：

* 📄 **一页速记 PDF / Markdown**
* 🧪 **AWS 考试陷阱题（IaaS vs PaaS 常混）**
* 🧠 **“关键词 → 秒选模型”反射表**
