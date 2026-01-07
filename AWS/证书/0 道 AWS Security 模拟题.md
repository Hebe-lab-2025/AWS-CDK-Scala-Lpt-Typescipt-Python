太好了 👍
我会一次性给你：

* ✅ **20 道 AWS Security 模拟题（含答案+简要解析）**
* ✅ **考试速记表格（高频服务 × 功能 × 关键词）**

全部是 **考试风格**，覆盖：
IAM、KMS、VPC、安全组、WAF、CloudTrail、GuardDuty、S3加密、组织与合规等。

---

# 🛡 AWS Security — 20 道模拟题（含答案与解析）

> 单选为主，难度 = SAA/SAP 真题风格

---

### 1️⃣ Which is AWS’s **primary service** for identity and access control?

A. KMS
B. IAM
C. Organizations
D. Cognito

✔ **Answer:** B

💡 IAM controls “who can access what”.

---

### 2️⃣ Which feature enforces **multi-factor authentication** for the root user?

A. IAM Policy
B. SCP
C. MFA
D. Access Analyzer

✔ **Answer:** C

💡 Root account **必须开启 MFA** 是最佳实践。

---

### 3️⃣ Which service **logs all API calls** in your account?

A. CloudWatch
B. CloudTrail
C. Config
D. Inspector

✔ **Answer:** B

💡 CloudTrail = 审计日志 / 谁做了什么。

---

### 4️⃣ Which provides **threat detection using machine learning**?

A. CloudTrail
B. GuardDuty
C. Security Hub
D. WAF

✔ **Answer:** B

💡 GuardDuty = 智能威胁检测。

---

### 5️⃣ Which controls **DDoS protection**?

A. WAF
B. GuardDuty
C. Shield
D. Inspector

✔ **Answer:** C

💡 Shield Standard = 自动保护
💡 Shield Advanced = 企业增强版

---

### 6️⃣ Which helps enforce **least privilege** across multiple accounts?

A. IAM Policy
B. Organizations SCP
C. VPC
D. GuardDuty

✔ **Answer:** B

💡 SCP 作用范围 = **组织级**

---

### 7️⃣ Which controls **network-level inbound/outbound traffic for instances**?

A. NACL
B. Security Group
C. Route Table
D. WAF

✔ **Answer:** B

💡 SG = 有状态
💡 放行返回流量

---

### 8️⃣ Which controls **subnet-level traffic**?

A. Security Group
B. NACL
C. VPC Peering
D. VPN

✔ **Answer:** B

💡 NACL = 无状态 + 有顺序

---

### 9️⃣ Which ensures **S3 objects cannot be publicly accessed accidentally**?

A. Bucket Policy
B. S3 ACL
C. S3 Block Public Access
D. Versioning

✔ **Answer:** C

💡 阻断一切公开访问配置

---

### 🔟 What encrypts S3, EBS, RDS data at rest?

A. IAM
B. KMS
C. Shield
D. WAF

✔ **Answer:** B

💡 KMS = 密钥管理服务

---

### 1️⃣1️⃣ Which encrypts data **in transit**?

A. TLS / HTTPS
B. S3 encryption
C. KMS
D. IAM

✔ **Answer:** A

💡 关键关键词：**in-transit**

---

### 1️⃣2️⃣ Which AWS service evaluates **security best practices**?

A. CloudTrail
B. Config
C. Trusted Advisor
D. GuardDuty

✔ **Answer:** C

💡 成本 + 安全 + 容错 + 性能

---

### 1️⃣3️⃣ Which service checks **resource configuration compliance**?

A. Config
B. CloudTrail
C. Inspector
D. GuardDuty

✔ **Answer:** A

💡 Config = 资源是否符合策略

---

### 1️⃣4️⃣ Which scans **EC2 for vulnerabilities**?

A. GuardDuty
B. Inspector
C. WAF
D. Shield

✔ **Answer:** B

💡 Inspector = 漏洞扫描

---

### 1️⃣5️⃣ IAM Role should be used instead of access keys primarily because?

A. Faster
B. Cheaper
C. Temporary credentials
D. Easier UI

✔ **Answer:** C

💡 安全核心词：**temporary**

---

### 1️⃣6️⃣ What prevents accidental S3 deletion?

A. KMS
B. Lifecycle policy
C. Versioning + MFA Delete
D. Replication

✔ **Answer:** C

---

### 1️⃣7️⃣ What enforces **web application firewall rules**?

A. Shield
B. WAF
C. GuardDuty
D. IAM

✔ **Answer:** B

💡 常考攻击：

* SQL injection
* XSS

---

### 1️⃣8️⃣ How to restrict S3 access **to only a specific VPC**?

A. IAM Policy
B. Bucket Policy
C. VPC Endpoint Policy
D. Security Group

✔ **Answer:** C

---

### 1️⃣9️⃣ What is a **shared responsibility model** example?

A. AWS patches EC2 OS
B. Customer encrypts S3 objects
C. AWS manages IAM users
D. AWS secures customer passwords

✔ **Answer:** B

💡 规则：

* AWS → cloud 的安全
* 客户 → cloud 中内容的安全

---

### 2️⃣0️⃣ How to detect **suspicious IAM activity** automatically?

A. IAM
B. GuardDuty
C. Shield
D. WAF

✔ **Answer:** B

---

# 📚 AWS Security — 高频考点速记表（中英双语）

> 可直接当考试小抄 🔥

| 类别     | 服务                       | 作用                    | 关键词                  |
| ------ | ------------------------ | --------------------- | -------------------- |
| 身份管理   | IAM                      | 用户 / 角色 / 策略          | least privilege      |
| 组织管理   | Organizations            | 多账号治理                 | SCP                  |
| 审计     | CloudTrail               | API 日志                | who did what         |
| 合规     | Config                   | 配置合规                  | evaluation           |
| 威胁检测   | GuardDuty                | 异常行为检测                | machine learning     |
| 漏洞扫描   | Inspector                | EC2 / ECR 扫描          | CVE                  |
| 安全汇总   | Security Hub             | 安全中心仪表盘               | findings             |
| DDoS   | Shield                   | 防御攻击                  | L3/L4                |
| WAF    | Web Application Firewall | 阻断 Web 攻击             | SQLi / XSS           |
| 加密     | KMS                      | 密钥管理                  | customer managed key |
| 数据加密   | S3 / EBS / RDS           | at rest encryption    | AES-256              |
| 传输加密   | HTTPS / TLS              | in transit encryption | SSL/TLS              |
| 网络安全   | VPC                      | 私有网络                  | isolation            |
| 子网     | NACL                     | 无状态过滤                 | subnet level         |
| 实例     | Security Group           | 有状态过滤                 | instance level       |
| 公共访问阻断 | S3 Block Public Access   | 禁止公开                  | bucket safety        |
| 多因子    | MFA                      | 双重认证                  | root required        |
| 账号隔离   | Multi-account            | blast radius          | least impact         |
| 端点安全   | VPC Endpoint             | 私网访问 S3/Dynamo        | no internet          |
| 最佳原则   | Shared Responsibility    | 责任共担                  | AWS vs Customer      |

---

## 🎯 你接下来想要：

* 🔹 再 20 题更难版（SAP 难度）
* 🔹 把这些整理成 **A4 PDF 速记卡**
* 🔹 模拟真实考试（倒计时 + 得分）
* 🔹 口语版面试问答练习
