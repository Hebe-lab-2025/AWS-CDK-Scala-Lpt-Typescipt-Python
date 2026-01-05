```
好的 😊 下面给你：

* ✅ 中文题目
* ✅ 标准参考答案
* ✅ 详细解析
* 👉 全部为**原创练习题**、只用于概念学习

---

# 🧭 第一部分：20 道概念练习题（中文）

---

### 1️⃣ 下列哪一个用于在 AWS 中创建**私有网络**？

A. IAM
B. VPC
C. S3
D. CloudFront

**答案：B**
**解析：**VPC（Virtual Private Cloud）提供用户在 AWS 中的专属网络环境。

---

### 2️⃣ 哪个服务可以签发**自动过期的临时凭证**？

A. IAM User
B. Access Key
C. AWS STS
D. Security Group

**答案：C**
**解析：**STS = Security Token Service，生成短期凭证。

---

### 3️⃣ 身份提供商（IdP）主要负责什么？

A. 存储镜像
B. 身份认证（登录验证）
C. 网络流量分发
D. 数据加密

**答案：B**
**解析：**IdP 核心功能是“你是谁”。

---

### 4️⃣ IAM Identity Center 的主要作用是：

A. 管理账单
B. 控制台外观
C. 集中式访问和权限管理
D. 对象存储

**答案：C**
**解析：**它是 AWS 的统一登录与权限中心（原 AWS SSO）。

---

### 5️⃣ 判断题

AWS 负责数据中心物理安全。

**答案：正确**
**解析：**属于 **security of the cloud**。

---

### 6️⃣ 判断题

安全组（Security Group）的配置属于客户责任。

**答案：正确**
**解析：**客户负责 **security in the cloud**。

---

### 7️⃣ 以下哪一个是常见的 IdP？

A. S3
B. Google Workspace
C. RDS
D. CloudWatch

**答案：B**
**解析：**IdP 负责企业统一账户登录。

---

### 8️⃣ 临时凭证通常与谁绑定？

A. IAM User
B. IAM Role
C. IAM Policy
D. KMS

**答案：B**
**解析：**角色 = 权限 + 临时凭证机制。

---

### 9️⃣ 在 VPC 中进行逻辑网络划分使用：

A. Subnet
B. AMI
C. Lambda
D. SNS

**答案：A**
**解析：**子网在 VPC 内进一步分区。

---

### 🔟 AWS STS 提供：

A. 长期 Access Key
B. Root 账号密码
C. 临时安全凭证
D. MFA 设备

**答案：C**
**解析：**临时 + 自动过期。

---

### 1️⃣1️⃣ 下列哪项是客户责任？

A. 机房门禁
B. 主机硬件维护
C. 应用层数据加密
D. 网络光纤铺设

**答案：C**
**解析：**客户负责自己放进去的数据与配置。

---

### 1️⃣2️⃣ 下列哪项**不属于** VPC 组成部分？

A. 子网
B. 路由表
C. Internet Gateway
D. IAM Policy

**答案：D**
**解析：**IAM Policy 属于身份系统，不是网络组件。

---

### 1️⃣3️⃣ 角色（IAM Role）主要用途：

A. 创建用户
B. 提供临时访问权限
C. 购买 EC2
D. 日志监控

**答案：B**

---

### 1️⃣4️⃣ IAM Identity Center 可以对接：

A. 只能 AWS 本地用户
B. 只能移动应用
C. 外部 IdP
D. 不能对接任何外部系统

**答案：C**
**解析：**可接 Okta、Google、Azure AD 等。

---

### 1️⃣5️⃣ 判断题

长期 Access Key 比临时凭证更安全。

**答案：错误**
**解析：**长期 key 风险更大不可控。

---

### 1️⃣6️⃣ 哪个可以在子网边界控制流量？

A. NACL
B. EC2
C. S3
D. EBS

**答案：A**

---

### 1️⃣7️⃣ “Security **of** the cloud” 由谁负责？

A. 客户
B. AWS

**答案：B**

---

### 1️⃣8️⃣ “Security **in** the cloud” 由谁负责？

A. 客户
B. AWS

**答案：A**

---

### 1️⃣9️⃣ 哪个服务可以**单点登录多个 AWS 账户**？

A. IAM Identity Center
B. EC2
C. Route 53
D. ECR

**答案：A**

---

### 2️⃣0️⃣ 哪个功能控制私网 + 公网出入口？

A. NAT Gateway
B. AMI
C. SNS
D. SQS

**答案：A**
**解析：**私有子网访问互联网用 NAT Gateway。

---

# 🎯 你要的重点总结

* ✔ VPC = 私有网络
* ✔ Subnet = 网络分区
* ✔ STS = 临时凭证
* ✔ Role = 授予临时权限
* ✔ IdP = 认证（你是谁）
* ✔ Identity Center = 授权（你能干嘛）
* ✔ Shared model = 云上责任划分

---

## 👍 如果你愿意，我还能继续

我可以继续为你生成：

* 📌 50 题考试风格练习题
* 🧭 超简版速背口诀
* 🧠 思维导图高清图
* 📝 PDF 版复习手册
* 🎤 面试题回答模板

```
```
Sure! Below are the **English versions**:

* ✅ Original practice questions
* ✅ Reference answers
* ✅ Clear explanations
* 🔒 Not tied to any protected assessments

---

# 🧭 Part 1 — 20 Concept Practice Questions (in English)

---

### 1️⃣ Which AWS service is used to create a private network?

A. IAM
B. VPC
C. S3
D. CloudFront

**Answer:** B
**Explanation:** VPC (Virtual Private Cloud) lets you build an isolated network in AWS.

---

### 2️⃣ Which service issues temporary security credentials that expire automatically?

A. IAM user
B. Access keys
C. AWS STS
D. Security Groups

**Answer:** C
**Explanation:** AWS STS = Security Token Service → generates temporary credentials.

---

### 3️⃣ What does an Identity Provider (IdP) do?

A. Stores EC2 images
B. Authenticates users (login identity)
C. Distributes network traffic
D. Encrypts database data

**Answer:** B
**Explanation:** IdP verifies **who you are**.

---

### 4️⃣ What is AWS IAM Identity Center primarily used for?

A. Billing management
B. UI customization
C. Centralized access and permissions management
D. Object storage

**Answer:** C

---

### 5️⃣ True or False

AWS is responsible for the physical security of data centers.

**Answer:** True
**Explanation:** That is **security of the cloud**.

---

### 6️⃣ True or False

Configuring Security Groups is the responsibility of the customer.

**Answer:** True
**Explanation:** That is **security in the cloud**.

---

### 7️⃣ Which of the following is an example of an IdP?

A. S3
B. Google Workspace
C. Amazon RDS
D. CloudWatch

**Answer:** B

---

### 8️⃣ Temporary security credentials are typically associated with:

A. IAM users
B. IAM roles
C. IAM policies
D. KMS keys

**Answer:** B
**Explanation:** Roles grant temporary permissions.

---

### 9️⃣ Which resource is used for logical network segmentation inside a VPC?

A. Subnet
B. AMI
C. Lambda
D. SNS

**Answer:** A

---

### 🔟 What does AWS STS provide?

A. Long-term access keys
B. Root user credentials
C. Short-term security credentials
D. EC2 SSH keys

**Answer:** C

---

### 1️⃣1️⃣ Which of the following is the customer’s responsibility?

A. Data center access control
B. Hardware maintenance
C. Application data encryption
D. Laying fiber cables

**Answer:** C

---

### 1️⃣2️⃣ Which of the following is **NOT** a component of a VPC?

A. Subnet
B. Route table
C. Internet Gateway
D. IAM policy

**Answer:** D
**Explanation:** IAM policy belongs to identity and access, not networking.

---

### 1️⃣3️⃣ What is the primary purpose of an IAM role?

A. Create user accounts
B. Provide temporary access permissions
C. Purchase EC2 instances
D. Monitor logs

**Answer:** B

---

### 1️⃣4️⃣ IAM Identity Center can integrate with:

A. Only AWS local users
B. Only mobile applications
C. External identity providers
D. No external systems

**Answer:** C

---

### 1️⃣5️⃣ True or False

Long-term access keys are safer than temporary credentials.

**Answer:** False

---

### 1️⃣6️⃣ Which resource can control traffic at the subnet boundary?

A. Network ACL (NACL)
B. EC2
C. S3
D. EBS

**Answer:** A

---

### 1️⃣7️⃣ “Security **of** the cloud” is the responsibility of:

A. The customer
B. AWS

**Answer:** B

---

### 1️⃣8️⃣ “Security **in** the cloud” is the responsibility of:

A. The customer
B. AWS

**Answer:** A

---

### 1️⃣9️⃣ Which AWS service supports single sign-on to multiple AWS accounts?

A. IAM Identity Center
B. EC2
C. CloudFormation
D. Amazon ECR

**Answer:** A

---

### 2️⃣0️⃣ Which service enables private subnets to access the internet?

A. NAT Gateway
B. AMI
C. SNS
D. EBS

**Answer:** A

---

# 🎯 Key Takeaways (English)

* VPC = private network
* Subnet = network segment
* STS = issues temporary credentials
* IAM Role = grants temporary permissions
* IdP = authentication (who you are)
* IAM Identity Center = authorization (what you can do)
* AWS vs Customer responsibilities = shared responsibility model

---

## If you’d like next:

✅ 50 English practice questions
✅ exam-style scenarios with diagrams
✅ interview answers
✅ flashcards
✅ PDF study sheet
```
