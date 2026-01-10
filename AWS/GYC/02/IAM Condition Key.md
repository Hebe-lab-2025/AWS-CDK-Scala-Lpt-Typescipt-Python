好，这一条我**一次性三件全做完**，而且全部是 **SAA-C03 可直接拿分版本**。
你可以把下面内容 **直接复制进 GitHub / Notion / 考前速记**。

---

# 🧠 IAM Condition Key 一页速记卡（SAA-C03 必会）

## ① `aws:SourceIp`

### 是什么

> **限制请求来源 IP / 网段**

```json
"Condition": {
  "IpAddress": {
    "aws:SourceIp": "203.0.113.0/24"
  }
}
```

### 典型使用

* 只允许公司内网访问
* 只允许 Bastion / VPN 出口 IP

### 考试反射

```text
Source IP / corporate network / office → aws:SourceIp
```

### ❌ 高频误区

* ❌ 用它限制 AWS 服务（错）
* ❌ 忽略 NAT / ALB 出口 IP

---

## ② `aws:PrincipalArn`

### 是什么

> **限制“是谁在调用”**（具体 User / Role）

```json
"Condition": {
  "StringEquals": {
    "aws:PrincipalArn": "arn:aws:iam::123456789012:role/AdminRole"
  }
}
```

### 典型使用

* 只允许某个 Role 访问资源
* 防止其他 IAM 实体误用权限

### 考试反射

```text
Only this role / specific IAM role → aws:PrincipalArn
```

### ❌ 高频误区

* ❌ 用 Resource ARN 代替（错）
* ❌ 忘了这是 **调用者**，不是被访问资源

---

## ③ `aws:MultiFactorAuthPresent`

### 是什么

> **是否使用 MFA 登录**

```json
"Condition": {
  "Bool": {
    "aws:MultiFactorAuthPresent": "true"
  }
}
```

### 典型使用

* 删除资源
* 修改安全配置
* 高风险操作

### 考试反射

```text
Sensitive / high-risk / extra security → MFA
```

### ❌ 高频误区

* ❌ 以为 MFA 是 IAM Policy（它是 Condition）
* ❌ 忘了是 `Bool` 类型

---

# 🧪 5 道 IAM + Condition · SAA-C03 陷阱题

---

### ❓Q1

> 只允许公司办公网络访问 S3 bucket，应该使用？

A. aws:RequestedRegion
B. aws:PrincipalArn
C. aws:SourceIp
D. aws:MultiFactorAuthPresent

✅ **答案：C**

---

### ❓Q2

> 只允许 AdminRole 删除 S3 对象，其他角色即使有权限也不行？

A. Resource ARN
B. aws:SourceIp
C. aws:PrincipalArn
D. aws:UserAgent

✅ **答案：C**

---

### ❓Q3

> 要求用户开启 MFA 才能删除 IAM User，怎么做？

A. IAM Group
B. Service Control Policy
C. aws:MultiFactorAuthPresent
D. aws:RequestedRegion

✅ **答案：C**

---

### ❓Q4（高频坑）

> 用户在 us-west-2 创建 bucket 被拒绝，策略中最可能有？

A. aws:SourceIp
B. aws:RequestedRegion
C. aws:PrincipalArn
D. aws:MultiFactorAuthPresent

✅ **答案：B**

---

### ❓Q5（真考风）

> 即使用户有 S3FullAccess，但在某些情况下仍被拒绝访问，最可能原因？

A. 没有 List 权限
B. Policy JSON 写错
C. Condition 不满足
D. Bucket Policy 缺失

✅ **答案：C**

---

# ✅ 改造你的 Policy：最小权限 + 考试满分版

## 🎯 设计目标

* ✅ 最小权限
* ✅ 考试标准写法
* ✅ Console / CLI 都能正常用
* ❌ 不给任何多余权限

---

## ✅【最终满分版本 Policy】

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowListBucketsForConsole",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    },
    {
      "Sid": "AllowListSpecificBucket",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::test-bucket-*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    },
    {
      "Sid": "AllowPutDeleteSpecificObjects",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::test-bucket-*/Operations_data.txt",
        "arn:aws:s3:::test-bucket-*/Public_data.txt"
      ],
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    }
  ]
}
```

---

## 🧠 为什么这是「考试满分版」？

### ✔ 拆分 List 权限（官方最佳实践）

* `ListAllMyBuckets` → 控制台必需
* `ListBucket` → 具体 bucket

### ✔ 没有 `s3:List*`（过大）

### ✔ 没有 `Resource:"*"` 用在对象操作

### ✔ Condition 一致、清晰、可考

---

## 🚨 SAA-C03 最终保命口诀

```text
• Condition = 最后一道闸门
• 权限不生效，先看 Condition
• Region / IP / MFA 是三大杀手锏
• 最小权限 > 一切
```

---

如果你愿意，我下一步可以直接帮你：

* 🔥 把 **IAM + Condition + Boundary + SCP** 做成一页「终极对照表」
* 🧪 出一套 **只考 IAM 的 20 题地狱卷**
* 📱 把这份速记卡做成 **手机锁屏版**

你只要说一句：**继续**。
