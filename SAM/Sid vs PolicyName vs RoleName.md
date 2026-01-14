## 🧠 Sid vs PolicyName vs RoleName ——一眼区分（**这是 IAM 面试/排错高频混淆点**）

| 名称             | 在哪出现                         | 是不是 AWS 资源名 | AWS 会不会用它做鉴权 | 真实作用                                  |
| -------------- | ---------------------------- | ----------- | ------------ | ------------------------------------- |
| **Sid**        | **Policy Statement 内部字段**    | ❌ 不是资源      | ❌ 不参与权限判断    | **人类可读标签**，用于**定位/调试/审计**             |
| **PolicyName** | IAM Policy（managed / inline） | ✅ 是资源名      | ❌ 不直接参与鉴权    | **标识这份 policy**（便于管理/引用）              |
| **RoleName**   | IAM Role                     | ✅ 是资源名      | ✅ **核心鉴权主体** | **谁在用权限**（Lambda / EC2 / User Assume） |

### 一句话记忆法（面试直接说）

* **RoleName = 谁在用权限**
* **PolicyName = 用的是哪份权限规则**
* **Sid = 规则里“哪一条”**

---

## 🧠 三者在一段真实 IAM Policy 里的位置

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadCoursesTable",          // 👈 Sid：这一条规则的名字（调试用）
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:Query"
      ],
      "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Courses"
    }
  ]
}
```

* 这段 policy **可能叫**：`PolicyName = CoursesReadPolicy`
* 它 **被 attach 到**：`RoleName = get-courses-lambda-role`
* 真正执行时：

  * **AWS 先看 Role**
  * **Role 里有哪些 Policy**
  * **Policy 里哪条 Statement 命中**
  * **Sid 只用来“告诉你是哪条”**

---

## 🧪 3 道 IAM Policy 易混淆判断题（真·面试风）

### ❓题 1

> IAM 判断是否允许一个请求时，会不会看 `Sid`？

**❌ 错误**

✅ 正确解释：

* AWS **完全不看 Sid**
* 只看：`Effect + Action + Resource + Condition`
* Sid 只给 **人类 debug / audit**

---

### ❓题 2

> 如果两个 Statement 的 Sid 相同，会导致冲突或覆盖吗？

**❌ 错误**

✅ 正确解释：

* Sid **不要求唯一**
* 冲突/覆盖只由 **Allow / Deny + 条件** 决定
* 但：**重复 Sid = 排错地狱（不推荐）**

---

### ❓题 3

> AccessDenied 报错里出现的 RoleName，一定是发请求的那个角色吗？

**✅ 正确（99% 场景）**

解释要点：

* 报错里的 ARN 通常是：

  ```text
  arn:aws:sts::ACCOUNT_ID:assumed-role/RoleName/SessionName
  ```
* **RoleName 就是当前执行主体**
* Lambda / EC2 / ECS 排错第一步：**确认是不是“这个角色”**

---

## 🔍 用真实 AccessDenied 报错示例：如何靠 Sid 精准定位问题

### 🚨 真实报错示例（DynamoDB）

```text
AccessDeniedException:
User: arn:aws:sts::123456789012:assumed-role/get-courses-lambda-role/GetCoursesFunction
is not authorized to perform: dynamodb:Query
on resource: arn:aws:dynamodb:us-west-2:123456789012:table/Courses
because no identity-based policy allows the dynamodb:Query action
```

---

## 🧠 **三步定位法（面试/实战通吃）**

### ✅ Step 1：锁定 **RoleName（谁在用权限）**

```text
assumed-role/get-courses-lambda-role/...
```

👉 当前 Lambda 用的是：**`get-courses-lambda-role`**

---

### ✅ Step 2：检查这个 Role attach 了哪些 Policy

你会看到类似：

* `AWSLambdaBasicExecutionRole`
* `CoursesReadPolicy`  👈 你自己写的

---

### ✅ Step 3：在 Policy 里用 **Sid 精准定位缺失规则**

假设你打开 `CoursesReadPolicy`，看到：

```json
{
  "Statement": [
    {
      "Sid": "AllowGetCourses",          // 👈 注意 Sid
      "Effect": "Allow",
      "Action": "dynamodb:GetItem",
      "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Courses"
    }
  ]
}
```

🔴 **问题一眼就出来了：**

* 报错说：`dynamodb:Query`
* Policy 里 Sid=`AllowGetCourses`
* 但 Action **只有 GetItem，没有 Query**

👉 **你应该补的不是“随便加权限”**，而是：

```json
{
  "Sid": "AllowQueryCourses",        // 👈 新 Sid，定位更清晰
  "Effect": "Allow",
  "Action": "dynamodb:Query",
  "Resource": "arn:aws:dynamodb:us-west-2:123456789012:table/Courses"
}
```

---

## 🧠 面试官最爱听的“排错总结句”

你可以直接背这一句：

> “When I see an AccessDenied error, I first identify the **assumed role** from the error message, then check which **policies are attached to that role**, and finally locate the **exact missing or mismatched permission by inspecting individual statements, often using the Sid for quick identification**.”

---

## 🔚 终极速记卡（给你压一句）

```text
RoleName → WHO
PolicyName → WHICH RULE SET
Sid → WHICH STATEMENT (for humans, not AWS)
```

如果你愿意，下一步我可以：

* 🔥 拿你 **SAM template 里的 IAM Policy**，帮你逐条补 **合理 Sid 命名**
* 🧪 出一套 **AccessDenied 盲排题**（只给报错，让你 30 秒定位问题）
