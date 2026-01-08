下面是一整套 **SAA-C03 实战级资料**，全部用 **Markdown + ASCII**，可以直接拿去当速记表 / 刷题模板。

---

# 🧭 一、Storage 决策树（ASCII / Markdown）

```text
【Step 1】数据类型是什么？
        |
        +-- 是否需要“挂载”到服务器？
        |       |
        |       +-- YES → Block Storage
        |       |          └─ Amazon EBS
        |       |
        |       +-- NO
        |
        +-- 是否需要“多台机器共享文件系统”？
        |       |
        |       +-- YES → File Storage
        |       |          ├─ Linux / NFS → Amazon EFS
        |       |          └─ Windows / 高性能 → Amazon FSx
        |       |
        |       +-- NO
        |
        +-- Object Storage
                └─ Amazon S3
                        |
                        +-- 高频访问 → S3 Standard
                        +-- 访问模式不确定 → S3 Intelligent-Tiering
                        +-- 低频访问 → S3 Standard-IA
                        +-- 归档（分钟级） → S3 Glacier
                        +-- 极低成本长期归档（小时级） → Glacier Deep Archive
```

📌 **考试用法**：
先做“数据类型三选一”，直接干掉 2/3 选项。

---

# ✅ 二、AWS 服务真假速查表（非常高频）

## 1️⃣ 根本不存在（看到直接排）

| 选项名                | 是否存在 | 说明               |
| ------------------ | ---- | ---------------- |
| S3 Archive-Fast    | ❌    | AWS 没有这个         |
| DynamoDB SQL       | ❌    | DynamoDB 不支持 SQL |
| EC2 Serverless     | ❌    | Serverless ≠ EC2 |
| RDS Object Storage | ❌    | RDS 是关系型         |

---

## 2️⃣ 服务存在，但**场景用错**（经典陷阱）

| 服务          | 常见错误用法 | 正确定位   |
| ----------- | ------ | ------ |
| Snowball    | 实时数据流  | 离线迁移   |
| Glacier     | 实时查询   | 归档     |
| EBS         | 多实例共享  | 单实例块存储 |
| S3          | 低延迟事务  | 对象存储   |
| NAT Gateway | 入站访问   | 私网出站   |

---

# 🧪 三、10 道「排除法专用」训练题（含答案）
```
### Q1

需要 **多 EC2 实例共享文件系统（Linux）**
A. EBS
B. S3
C. EFS
D. Glacier

✅ **答案：C**
❌ A：不能共享
❌ B：不是文件系统
❌ D：归档

---

### Q2

需要 **sub-millisecond latency** 读热点数据
A. RDS
B. DynamoDB
C. DynamoDB + DAX
D. Snowball

✅ **答案：C**

---

### Q3

最小运维、突发流量、无需服务器
A. EC2 + ASG
B. Lambda
C. ECS on EC2
D. Beanstalk

✅ **答案：B**

---

### Q4

长期合规归档、访问频率极低、最低成本
A. S3 Standard
B. S3 IA
C. Glacier
D. Glacier Deep Archive

✅ **答案：D**

---

### Q5

Windows 文件共享（SMB）
A. EFS
B. FSx for Windows
C. S3
D. EBS

✅ **答案：B**

---

### Q6

实时流式日志采集
A. Snowball
B. SQS
C. Kinesis
D. Glacier

✅ **答案：C**

---

### Q7

EC2 私有子网访问互联网更新补丁
A. IGW
B. NAT Gateway
C. VPC Endpoint
D. NACL

✅ **答案：B**

---

### Q8

数据库连接超时，最先检查
A. RDS CPU
B. 表索引
C. Security Group
D. Storage size

✅ **答案：C**

---

### Q9

静态网站、全球低延迟
A. EC2
B. S3 + CloudFront
C. RDS
D. EFS

✅ **答案：B**

---

### Q10

强一致、关系型、事务
A. DynamoDB
B. S3
C. RDS
D. ElastiCache

✅ **答案：C**
```
---

# 🔁 四、把方法推广到其他领域（决策树思维）

---

## 🖥️ Compute 决策树（简化）

```text
是否需要管服务器？
    |
    +-- NO → Lambda
    |
    +-- YES
        |
        +-- 容器？
        |     |
        |     +-- 不想管 EC2 → ECS Fargate
        |     +-- 需要控制 → ECS / EKS
        |
        +-- 传统应用 → EC2 / ASG
```

---

## 🗄️ Database 决策树（简化）

```text
是否关系型？
    |
    +-- YES → RDS / Aurora
    |
    +-- NO
        |
        +-- Key-Value / 高扩展 → DynamoDB
        +-- 缓存 / 亚毫秒 → ElastiCache
        +-- 搜索 → OpenSearch
```

---

## 🌐 Networking 决策树（简化）

```text
是否公网访问？
    |
    +-- YES → ALB / CloudFront
    |
    +-- NO
        |
        +-- 私网出站 → NAT Gateway
        +-- AWS 内部访问 → VPC Endpoint
```

---

# 🧠 终极考试口诀（建议背）

> **分类 → 套约束 → 排除假项 → 剩下即答案**

---

如果你愿意，我可以下一步直接帮你：
```                       
* 🔹 做一套 **完整 SAA-C03 决策树 PDF**
* 🔹 把这些转成 **Notion / Obsidian 笔记模板**
* 🔹 出一轮 **“故意挖坑”的高难模拟题**
* 🔹 帮你训练 **30 秒快速定位正确答案的直觉**
```
