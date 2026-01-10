下面这组内容 **100% 命中你刚才那个 S3 Bucket Policy 场景**，也是 **AWS 考试 + 面试** 的高频雷区。

---

## 🔐 1️⃣ 把 `Principal: "*"` 改成 **只允许 EC2 / Lambda（标准写法）**

### ❌ 错误 / 高风险写法（你看到的）

```json
"Principal": "*"
```

👉 **含义**：全世界任何人（匿名用户）
👉 **结果**：Bucket 直接变公网（即使你“只是想给 EC2 用”）

---

## ✅ 正确做法一：**只允许“某个 EC2 Role”访问 S3（最推荐）**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowEC2RoleAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:role/MyEC2Role"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### 🧠 这句话在“人话”里的意思

> **“只有挂了 MyEC2Role 的 EC2，才能读这个 bucket”**

📌 核心思想：

* **EC2 ≠ Principal**
* **EC2 上的 IAM Role 才是 Principal**

---

## ✅ 正确做法二：**只允许 Lambda Role 访问 S3**

```json
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::123456789012:role/MyLambdaRole"
  },
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

👉 Lambda 本身也不是 Principal
👉 **Lambda Execution Role 才是**

---

## ⚠️ 常见误区（考试/面试必考）

❌ 错误写法（很多人会想当然）：

```json
"Principal": {
  "Service": "ec2.amazonaws.com"
}
```

❗这是 **Trust Policy** 用法，不是 Bucket Policy 用法
👉 **Bucket Policy 的 Principal = AWS 身份（Role / User / Account）**

---

## 🧪 补充：**如果你真要“同时允许 EC2 + Lambda”**

```json
"Principal": {
  "AWS": [
    "arn:aws:iam::123456789012:role/MyEC2Role",
    "arn:aws:iam::123456789012:role/MyLambdaRole"
  ]
}
```

---

## 🧪 2️⃣ IAM Policy vs Bucket Policy 对比图（ASCII）

```text
            ┌──────────────┐
            │   IAM User   │
            │  / Role      │
            └──────┬───────┘
                   │
         IAM Policy │   （我“能不能”访问 S3？）
                   │
                   v
            ┌──────────────┐
            │   S3 Bucket  │
            └──────┬───────┘
                   │
      Bucket Policy │   （我“允不允许你”访问？）
                   │
                   v
               Object
```

### 🧠 一句话模型（一定要背）

> **IAM Policy：你“想不想 / 能不能”**
> **Bucket Policy：资源“让不让你”**

📌 **最终生效规则 = 两边都 Allow**

---

## 🧠 3️⃣ S3 Public Access Block —— 高频坑题合集（必秒）

### 🧪 坑题 1

**Q：Bucket Policy 已经写了 `Principal: "*"`，但访问还是 403，为什么？**

✅ 正解：

> **S3 Public Access Block 还开着**

📌 Public Access Block：

* 是 **账号级 / bucket 级的“总闸”**
* **优先级高于 Bucket Policy**

---

### 🧪 坑题 2

**Q：我只是想让 EC2 访问，为什么 AWS 说“public bucket”？**

✅ 正解：

> 只要 Bucket Policy 里出现 `Principal: "*"`，
> AWS 就认为你在“尝试公开”，**不管你真实意图**

---

### 🧪 坑题 3（考试最爱）

**Q：以下哪种方式“最安全”地让 EC2 访问私有 S3？**

选项里如果有：

* ✅ **IAM Role + Bucket Policy（指定 Role ARN）**
* ❌ Access Key 写在 EC2 上
* ❌ 公网 bucket + IP 白名单

👉 **秒选：IAM Role**

---

## 🧠 终极秒选口诀（刻进肌肉记忆）

> 🔐 **S3 不认 EC2 / Lambda，只认 IAM Role**
> 🚫 **看到 `Principal: "*"` = 公网风险**
> 🧱 **Public Access Block 是“最后一道铁门”**

---

如果你愿意，下一步我可以直接帮你做三选一强化训练：
1️⃣ 出 **10 道 S3 Policy 陷阱选择题（真考风格）**
2️⃣ 帮你把**你现在那个 Bucket Policy** 改成「考试 + 生产都完美」版本
3️⃣ 做一张 **S3 权限决策树速记卡（IAM / Bucket / Block 一页搞定）**
