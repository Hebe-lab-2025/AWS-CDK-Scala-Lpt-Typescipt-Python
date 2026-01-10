下面给你一整套 **👉 可直接复制到 GitHub 的 Markdown 内容**，包含你要的 **3 个部分**，而且是**AWS 考试取向（SAA / DVA / SOA 通用）**。

你可以 **原样粘贴到 README.md** 使用。

---

# 🔐 AWS IAM 一页速记卡（Exam Ready）

> 目标：**看到关键词，立刻选对 IAM 组件**

---

## 一、IAM 核心组件关系图（ASCII 图）

```text
            ┌───────────────┐
            │   IAM Policy  │
            │ (Permissions) │
            └───────▲───────┘
                    │ attached to
        ┌───────────┴───────────┐
        │                       │
┌───────┴───────┐       ┌───────┴───────┐
│   IAM User    │       │   IAM Role    │
│ (Human/User) │       │ (AWS Service) │
└───────▲───────┘       └───────▲───────┘
        │ belongs to                    │ assumed by
┌───────┴───────┐               ┌───────┴────────┐
│  IAM Group    │               │ AWS Services   │
│ (Users only) │               │ (EC2/Lambda)  │
└───────────────┘               └────────────────┘
```

---

## 二、每个组件一句话解释（必须背）

### 👤 IAM User

* **代表一个“人”或“长期账号”**
* 有 **Access Key / Password**
* ❌ 不推荐给 EC2 / Lambda 用

📌 关键词：**human / developer / admin**

---

### 👥 IAM Group

* **用户的集合**
* 权限通过 **Policy** 统一管理
* ❌ Group 不能包含 Role

📌 关键词：**manage users at scale**

---

### 🎭 IAM Role

* **给 AWS 服务 / 外部身份使用**
* **没有长期凭证**
* 使用 **STS 临时凭证**
* ✅ EC2 / Lambda / ECS 必选

📌 关键词：**temporary credentials / service-to-service**

---

### 📜 IAM Policy

* **真正定义“能做什么”**
* JSON 格式
* 可以 attach 给：

  * User
  * Group
  * Role

📌 关键词：**Allow / Deny / Action / Resource**

---

## 三、🧠 一页「关键词 → 秒选组件」反射表（核心）

| 题目关键词                              | 秒选         |
| ---------------------------------- | ---------- |
| **human user**                     | IAM User   |
| **multiple users same permission** | IAM Group  |
| **EC2 access S3**                  | IAM Role   |
| **Lambda access DynamoDB**         | IAM Role   |
| **temporary credentials**          | IAM Role   |
| **least privilege**                | IAM Policy |
| **long-term access key**           | IAM User   |
| **cross-account access**           | IAM Role   |
| **no credentials stored**          | IAM Role   |

---

## 四、🧪 AWS IAM 考试陷阱题（5 道高频）

---

### ❌ 陷阱题 1

**题目**：

> An EC2 instance needs to access an S3 bucket securely.

❌ 错误选项：

* Create IAM User and store access key on EC2

✅ 正确答案：
👉 **Attach IAM Role to EC2**

📌 原因：

* EC2 ≠ Human
* 不要长期 Key

---

### ❌ 陷阱题 2

**题目**：

> You want to manage permissions for 20 developers.

❌ 错误选项：

* Attach policy to each IAM User

✅ 正确答案：
👉 **Create IAM Group + attach policy**

📌 原因：

* Group = 批量管理

---

### ❌ 陷阱题 3

**题目**：

> An application needs temporary access to AWS resources.

❌ 错误选项：

* IAM User with access key

✅ 正确答案：
👉 **IAM Role + STS**

📌 原因：

* temporary = Role

---

### ❌ 陷阱题 4

**题目**：

> A Lambda function needs DynamoDB access.

❌ 错误选项：

* Store access key in Lambda environment variables

✅ 正确答案：
👉 **Attach IAM Role to Lambda**

📌 原因：

* Lambda 永远用 Role

---

### ❌ 陷阱题 5

**题目**：

> Grant the minimum required permission.

❌ 错误选项：

* AdministratorAccess

✅ 正确答案：
👉 **Custom IAM Policy**

📌 原因：

* least privilege

---

## 五、IAM 考试“反直觉”总规则（必背）

```text
❗ 能用 Role，就不要用 User
❗ AWS Service 永远不用 Access Key
❗ 权限 ≠ User，本质是 Policy
❗ temporary = Role
❗ human = User
```

---

## 六、30 秒考前自检清单

* 是人？还是 AWS 服务？
* 要不要长期 Key？
* 有没有提 temporary / secure / least privilege？
* 有没有 EC2 / Lambda / Cross-account？

---

## ✅ 一句话终极总结（面试 / 考试通用）

> IAM User 用于人类访问，IAM Group 用于批量管理用户权限，IAM Role 用于 AWS 服务或临时访问，IAM Policy 定义具体权限规则。

---

如果你愿意，下一步我可以继续给你（同样 Markdown 可复制）：

* 🔥 **IAM Policy JSON 模板（Allow / Deny / 条件）**
* 🧪 **10 道 IAM + S3 / EC2 混合陷阱题**
* 🧠 **IAM + S3 + KMS 联合决策树**

你直接说一句：
👉 **“来 Policy 模板” / “再来 10 道陷阱题” / “做联合决策树”**
