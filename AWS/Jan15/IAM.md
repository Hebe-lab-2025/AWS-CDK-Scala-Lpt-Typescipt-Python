### ✅ 正确答案

**A. True**

---

### 🧠 一句话记忆法（考场秒选）

> **IAM Policy 里可以用 `Action` 或 `NotAction`，甚至在同一个 Policy 的不同 Statement 里同时存在**

---

### 🔍 关键点拆解（避免误解）

* **同一个 Policy 文档**：

  * ✅ 可以同时包含 `Action` 和 `NotAction`
* **同一个 Statement**：

  * ❌ 不能同时写 `Action` 和 `NotAction`
  * 二选一

**合法示例（Policy 级别）：**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "NotAction": "iam:*",
      "Resource": "*"
    }
  ]
}
```

---

### 🧪 真考陷阱提醒

* 很多人误以为 **Policy 里只能二选一** → ❌
* 正确理解是：
  **Statement 级别不能混用，Policy 级别可以共存**

---

### 📌 超短口诀

```
Policy: Action + NotAction OK
Statement: only one
```

需要的话，我可以继续 **第 4 题 IAM Quiz**，或者直接给你 **5 道 SAA-C03 IAM 陷阱真题**。


### ✅ 正确答案

**C. Condition**

---

### 🧠 一句话记忆法（考场秒选）

> **Condition 是可选的，其它都是“几乎必需”**

---

### 🔍 逐个选项快速拆解

| 选项               | 是否可选 | 原因                         |
| ---------------- | ---- | -------------------------- |
| A. Action        | ❌    | 必须有（或用 `NotAction` 替代）     |
| B. Effect        | ❌    | **必须**（Allow / Deny）       |
| **C. Condition** | ✅    | **可选**，用于附加限制（IP、时间、MFA 等） |
| D. Resource      | ❌    | 大多数情况下必须（极少数服务例外）          |

---

### 🧪 真考陷阱提醒（SAA-C03 常见）

* 没写 `Condition` ≠ 不安全
* `Condition` 是 **“加锁”用的**，不是基本结构必需项
* 基本 IAM Policy 结构是：

```
Effect + Action/NotAction + Resource
(+ Condition 可选)
```

---

### 📌 超短口诀

```
Condition = optional
Effect / Action / Resource = core
```

如果你愿意，我可以继续 **第 5 题 IAM Quiz**，或者直接升级到 **SAA-C03 IAM 场景题**（比如 MFA / IP 限制 / 跨账号访问）。

### ✅ 正确答案

**D. 12 hours**

---

### 🧠 一句话记忆法（考场秒选）

> **IAM Role 会话最长 = 12 小时**

---

### 🔍 关键点拆解

* **IAM Role session**

  * **默认**：1 hour
  * **最大可配置**：**12 hours**
* 题目问的是 **maximum duration**，不是默认值

---

### 🧪 逐个选项快速排除

| 选项              | 说明         |
| --------------- | ---------- |
| A. 1 hour       | ❌ 默认值，不是最大 |
| C. 4 hours      | ❌          |
| B. 16 hours     | ❌ 超过上限     |
| **D. 12 hours** | ✅ 正确       |

---

### 📌 超短口诀

```
Role session:
Default = 1h
Max = 12h
```

如果你愿意，我可以继续 **IAM Quiz 7–10**，或者直接切到 **SAA-C03 IAM 场景大题**（AssumeRole、STS、跨账号访问）。


### ✅ 正确答案

**A. Trust policy**

---

### 🧠 一句话记忆法（考场秒选）

> **创建 IAM Role 必须先有 Trust Policy**

---

### 🔍 选项快速拆解

| 选项                   | 是否必需  | 原因                                   |
| -------------------- | ----- | ------------------------------------ |
| **A. Trust policy**  | ✅ 必须  | 定义 **谁可以 assume 这个 role**（Principal） |
| B. Permission policy | ❌ 非必需 | 可以 **创建后再 attach**                   |

---

### 🧪 真考陷阱提醒（SAA-C03）

* **没有 Trust Policy = Role 根本无法被使用**
* Permission Policy 决定 **能做什么**
* Trust Policy 决定 **谁能用**

---

### 📌 超短口诀

```
Create Role → Trust first
Do actions → Permission later
```

如果你继续，我可以把 **IAM Role = Trust vs Permission** 画成一张 **考场秒判图**，或者直接进入 **Quiz 8（STS / AssumeRole 场景题）**。


### ✅ 正确答案

**B. True**

---

### 🧠 一句话记忆法（考场秒选）

> **IAM Access Analyzer = Regional service**

---

### 🔍 为什么是 True？

* **IAM 本身是 Global service**
* 但 **IAM Access Analyzer 是 Regional**

  * 每个 **Region 单独启用**
  * 分析的是该 **Region 内资源**（如 S3、IAM Role、KMS、Lambda 等）的访问情况
* 在控制台中你会看到：**按 Region 切换 Access Analyzer**

---

### 🧪 SAA-C03 高频陷阱

* ❌ “IAM 是 Global → 所有 IAM 相关都是 Global” → **错**
* ✅ **Access Analyzer = Regional**
* 类似对比：

  * IAM User / Role / Policy → **Global**
  * Access Analyzer → **Regional**

---

### 📌 超短口诀

```
IAM = Global
Access Analyzer = Regional
```

如果你愿意，我可以继续 **Quiz 9–12（IAM + Security 真考风）**，或者帮你做一张 **Global vs Regional 安全服务对照速记表**（SAA-C03 超好用）。


### ✅ 正确答案

**B. Permission boundary**

---

### 🧠 一句话记忆法（考场秒选）

> **给 IAM User / Role 设“权限上限” → Permission Boundary**

---

### 🔍 逐个选项快速拆解

| 选项                         | 是否用于“权限上限” | 原因                    |
| -------------------------- | ---------- | --------------------- |
| A. Session policy          | ❌          | 临时 **缩小** 会话权限，不是长期上限 |
| **B. Permission boundary** | ✅          | 明确规定 **最大可拥有的权限集合**   |
| C. Trust policy            | ❌          | 决定 **谁能 assume role** |
| D. Permission policy       | ❌          | 决定 **实际允许的权限**，不是上限   |

---

### 🧪 SAA-C03 高频陷阱

* **Effective permissions = Permission policy ∩ Permission boundary**
* Boundary 只做一件事：**限制最大权限**
* Boundary **不会单独授予权限**

---

### 📌 超短口诀

```
Upper bound → Permission Boundary
Who can assume → Trust Policy
What you can do → Permission Policy
```

如果你继续，我可以直接给你 **Quiz 10（Session policy vs Boundary 场景题）**，这是 SAA-C03 非常爱考的对比点。


### ✅ 正确答案

**B. AWS Organizations**

---

### 🧠 一句话记忆法（考场秒选）

> **多账号统一管理 → AWS Organizations**

---

### 🔍 逐个选项快速拆解

| 选项                       | 是否用于多账号管理 | 原因                          |
| ------------------------ | --------- | --------------------------- |
| A. Access Analyzer       | ❌         | 分析资源访问，不管账号结构               |
| **B. AWS Organizations** | ✅         | 创建/管理 **多个 AWS 账号**，统一账单与策略 |
| C. IAM Identity Center   | ❌         | 做 **统一登录（SSO）**，不是账号管理      |
| D. Amazon Cognito        | ❌         | 面向应用的 **用户身份认证**（App 用户）    |

---

### 🧪 SAA-C03 高频考点补充

AWS Organizations 能做什么（考试爱考）：

* ✅ 管理多个 AWS 账号
* ✅ Consolidated Billing（统一账单）
* ✅ Service Control Policy（SCP）
* ✅ OU（组织单元）层级管理

---

### 📌 超短口诀

```
Multi-account → AWS Organizations
SSO → IAM Identity Center
App users → Cognito
```

如果你愿意，我可以直接帮你 **复盘 Quiz 1–10 的“秒选暗号表”**，或者继续出 **Quiz 11–15（SAA-C03 安全真考风）**。


