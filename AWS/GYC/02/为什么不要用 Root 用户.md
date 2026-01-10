下面我用**中文 + 安全最佳实践 + 新手/考试友好**的方式，帮你把 **为什么要创建 IAM 用户 + 如何创建 IAM 用户** 一次讲清楚 👇

---

## 一、为什么**不要**用 Root 用户？

![Image](https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/intro-diagram%20_policies_800.png)

![Image](https://cybr.com/wp-content/uploads/2024/02/iam-best-practices-banner.jpg)

![Image](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2017/07/06/ebs.png)

### Root 用户是什么？

* 创建 AWS 账号时**自动生成**
* **只有 1 个**
* 对账号内**所有资源拥有完全、不可限制的权限**

### 为什么不推荐使用 Root 用户？

❌ **权限太大**

* 一旦凭证泄露 = 整个 AWS 账号沦陷

❌ **不可审计**

* 无法区分是谁做了操作
* 不利于审计、追责、合规

❌ **安全最佳实践明确禁止日常使用**

📌 **Root 用户只应该做 3 件事：**

1. 创建 IAM 用户
2. 开启 MFA
3. 账号级设置（如关闭账号）

---

## 二、正确做法：使用 IAM 用户（Least Privilege）

### 核心原则：**最小权限原则（Least Privilege）**

👉 **给用户“刚好够用”的权限，不多不少**

好处：

* ✅ 降低误操作风险
* ✅ 防止权限滥用
* ✅ 每个操作都可追踪

---

## 三、什么是 IAM？

**AWS Identity and Access Management（IAM）** 是 AWS 的身份与权限管理服务，用来：

* 👤 管理 **谁** 可以访问 AWS（User / Role / Group）
* 🔐 管理 **能做什么**（Policy）
* 📊 记录 **谁在什么时候做了什么**

---

## 四、创建 IAM 用户的步骤（标准流程）

![Image](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2016/11/02/RMoncur1Phase0.png)

![Image](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2016/11/02/RMoncur4Phase0.png)

![Image](https://cloudiofy.com/wp-content/uploads/2022/08/aws-resource-permission-types.png)

### Step 1️⃣：登录 AWS 控制台（Root 一次性）

* 使用 **Root 用户** 登录
* 进入 **IAM 控制台**

---

### Step 2️⃣：创建用户

1. 点击 **Users → Create user**
2. 输入用户名（如：`dev-user`）

---

### Step 3️⃣：选择访问方式

* ✔ **AWS Management Console access**（网页登录）
* 或 ✔ **Access key**（CLI / SDK）

👉 实际工作中常见：

* 人：Console
* 程序：Access Key

---

### Step 4️⃣：分配权限（最关键）

三种方式：

1. **加入用户组（推荐）**
2. 直接附加策略
3. 从现有用户复制权限

📌 **推荐做法**：
👉 **先创建 Group → 再把 User 加进去**

---

### Step 5️⃣：附加权限策略（Policy）

* 使用 AWS 托管策略（如 `AmazonS3ReadOnlyAccess`）
* 或自定义最小权限策略

---

### Step 6️⃣：创建完成 & 保存凭证

* 下载 Access Key（只显示一次）
* 或设置初始密码

---

## 五、IAM 用户 vs Root 用户（一眼区分）

| 对比项  | Root 用户 | IAM 用户 |
| ---- | ------- | ------ |
| 数量   | 只能 1 个  | 可创建多个  |
| 权限   | 无限制     | 可精确控制  |
| 审计   | ❌ 不清晰   | ✅ 可追踪  |
| 日常使用 | ❌ 禁止    | ✅ 推荐   |

---

## 六、考试 & 面试高频结论 🧠

* **Never use root user for daily tasks**
* **Always follow least privilege**
* **Use IAM users / groups / roles**
* **Enable MFA for root & IAM**

📌 **考试关键词触发**：

* *security best practice* → IAM
* *audit / traceability* → IAM
* *least privilege* → IAM Policy

---

如果你愿意，我可以继续帮你：

* 🔐 **画一张 IAM（User / Group / Role / Policy）关系图**
* 🧪 **出 5 道 AWS IAM 考试陷阱题**
* 🧠 **把 IAM 做成“一页速记卡”**
