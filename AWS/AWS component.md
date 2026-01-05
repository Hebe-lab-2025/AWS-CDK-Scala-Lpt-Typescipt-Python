Great—here are learning-only materials that **do not reference or use any protected assessments**. They are original practice questions and concept summaries.

---

# ✅ Part 1 — 20 Concept-Only Practice Questions (Multiple-choice & T/F)

These check **understanding only**, not any course content.

---

### 1️⃣ Which AWS component creates an isolated private network?

A. IAM
B. VPC
C. S3
D. CloudFront

---

### 2️⃣ What issues temporary credentials that automatically expire?

A. IAM user
B. Access keys
C. AWS STS
D. Security Groups

---

### 3️⃣ Which best describes an Identity Provider (IdP)?

A. A database service
B. A service that stores EC2 AMIs
C. A service that verifies user identity
D. A logging service

---

### 4️⃣ IAM Identity Center is mainly used for:

A. Billing alerts
B. Multi-region networking
C. Centralized access & permissions management
D. Encrypting S3 objects

---

### 5️⃣ True or False

AWS manages physical data centers under the shared responsibility model.

---

### 6️⃣ True or False

Customers are responsible for configuring Security Groups.

---

### 7️⃣ Which is an example of an IdP?

A. S3
B. Google Workspace
C. Route 53
D. CloudTrail

---

### 8️⃣ Temporary credentials usually belong to:

A. IAM users
B. IAM roles
C. Security Groups
D. Elastic IPs

---

### 9️⃣ Which AWS service creates **logical network segmentation** inside a VPC?

A. Subnet
B. AMI
C. SNS
D. Lambda

---

### 🔟 What does AWS STS provide?

A. Permanent access keys
B. Database credentials
C. Short-term security credentials
D. EC2 passwords

---

### 1️⃣1️⃣ Which side is customer responsibility?

A. Hypervisor patching
B. Physical security doors
C. Encrypting application data
D. Fiber network installation

---

### 1️⃣2️⃣ Which is **NOT** part of VPC networking?

A. Route tables
B. Internet Gateway
C. Subnets
D. IAM policies

---

### 1️⃣3️⃣ A role is primarily used to:

A. Create users
B. Provide temporary permissions
C. Buy EC2 capacity
D. Monitor logs

---

### 1️⃣4️⃣ IAM Identity Center can integrate with:

A. Only AWS accounts
B. Only mobile apps
C. External IdPs
D. None

---

### 1️⃣5️⃣ True or False

Long-term access keys are safer than temporary credentials.

---

### 1️⃣6️⃣ Which resource isolates workloads at subnet level?

A. NACL
B. VPC
C. EBS
D. IAM Group

---

### 1️⃣7️⃣ Which best completes the sentence?

“Security **of** the cloud is handled by ______.”
A. The customer
B. AWS

---

### 1️⃣8️⃣ Which best completes the sentence?

“Security **in** the cloud is handled by ______.”
A. The customer
B. AWS

---

### 1️⃣9️⃣ Which service helps **single sign-on into multiple AWS accounts**?

A. IAM Identity Center
B. EC2
C. CloudFormation
D. ECR

---

### 2️⃣0️⃣ Which feature can restrict inbound/outbound traffic at subnet border?

A. IAM policy
B. NACL
C. EBS volume
D. S3 bucket policy

---

## 🧩 Answer Key (self-check)

1B  2C  3C  4C  5T
6T  7B  8B  9A  10C
11C 12D 13B 14C 15F
16A 17B 18A 19A 20B

---

# 🧪 Part 2 — Mock-Exam-Style Simulation Questions

These mimic exam *tone*, but are **fully original questions**.

---

### 🧭 Scenario 1

A company wants employees to log into AWS using their **corporate Google accounts**, without creating IAM users.

Which combination fits?

* Identity is verified by: ______
* Permissions are granted by: ______

Options:
A. IdP verifies identity; IAM Identity Center grants permissions
B. IAM grants identity; VPC grants permissions
C. STS verifies identity; EC2 grants permissions
D. S3 verifies identity; IAM grants permissions

---

### 🧭 Scenario 2

An application running on EC2 needs **temporary access** to S3 without storing keys.

What should be used?

A. IAM role
B. IAM group
C. Root user
D. EC2 key pair

---

### 🧭 Scenario 3

A company wants to isolate workloads by IP range, subnets, routing rules, and gateways.

Which AWS service should they design first?

A. IAM
B. Route 53
C. VPC
D. CloudTrail

---

### 🧠 Scenario Answers

1A
2A
3C

---

# 🧭 Part 3 — Text-based Mind Map (ASCII)

```
AWS Core Concepts
|
+-- Shared Responsibility Model
|     |-- AWS: security OF the cloud
|     |     |-- data centers
|     |     |-- hardware
|     |     |-- virtualization
|     |
|     |-- Customer: security IN the cloud
|           |-- data encryption
|           |-- IAM configuration
|           |-- network rules
|
+-- Identity & Access
|     |
|     +-- Identity Provider (IdP)
|     |     |-- Google / Okta / Azure AD
|     |     |-- Authentication (who you are)
|     |
|     +-- IAM Identity Center
|           |-- Authorization (what you can do)
|           |-- SSO for AWS accounts/apps
|
+-- Credentials
|     |
|     +-- Temporary Credentials
|           |-- AWS STS
|           |-- IAM Role
|           |-- expire automatically
|
+-- Networking
      |
      +-- VPC
      |     |-- subnets
      |     |-- route tables
      |     |-- gateways
      |
      +-- Security Tools
            |-- Security Groups
            |-- NACLs
```

---

## 🎯 Want next steps?

I can also generate:

🔹 flashcards
🔹 30-question full practice test
🔹 real-world architecture scenarios
🔹 interview-style Q&A
🔹 simplified Chinese/English bilingual notes
