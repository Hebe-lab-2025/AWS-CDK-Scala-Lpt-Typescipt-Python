下面我把你这个 **Lab（Permissions Boundary 作为 Identity Policy 的子集）**
用**“发生了什么 → 为什么 → 考试怎么秒选”**三步给你讲清楚，**直接对照你给的 JSON**。

---

# 🔐 Permissions Boundary = Identity Policy 的「硬上限」（子集场景）

## 一句话先给结论（这道 Lab 的核心）

> **最终生效权限 = Identity-based Policy ∩ Permissions Boundary**
> 👉 Identity 里多出来的权限，**全部作废**

---

## 🧩 先看你给的 Permissions Boundary JSON（关键点）

```json
{
  "Statement": [
    {
      "Action": "s3:List*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"
        }
      }
    },
    {
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

### 🧠 Boundary **允许的事情只有三类**

1. ✅ `s3:List*`（us-east-1）
2. ✅ 上传 / 删除 **Operations_data.txt**
3. ✅ 上传 / 删除 **Public_data.txt**

❌ **不允许的（即使 Identity Policy 允许）**

* 创建 bucket
* 删除 bucket
* 操作 `Management_data.txt`

---

## 🧪 为什么 Lab 里会出现这些现象？

### ❌ 现象 1：**无法创建 S3 bucket**

> 即使 Identity Policy 里有 `s3:CreateBucket`

**原因（考试必考）：**

* Permissions Boundary **没有** `s3:CreateBucket`
* 👉 最终权限 = 交集
* 👉 **CreateBucket ∉ Boundary → 被直接拒绝**

---

### ❌ 现象 2：**Management_data.txt 无法上传**

> Identity Policy 允许
> Boundary 不允许

```text
Identity: PutObject → Management_data.txt
Boundary: ❌ 没有
```

👉 **交集为空 → 拒绝**

---

### ✅ 现象 3：Operations / Public 文件可以上传

```text
Identity: 允许
Boundary: 允许
```

👉 **交集存在 → 成功**

---

## 📌 用一张“权限对照表”秒懂

| 操作                      | Identity Policy | Boundary | 最终 |
| ----------------------- | --------------- | -------- | -- |
| CreateBucket            | ✅               | ❌        | ❌  |
| Put Public_data.txt     | ✅               | ✅        | ✅  |
| Put Operations_data.txt | ✅               | ✅        | ✅  |
| Put Management_data.txt | ✅               | ❌        | ❌  |
| List Bucket             | ✅               | ✅        | ✅  |

---

## 🧠 为什么 AWS 要你这样设计 Boundary？

### 🎯 真正目的（非常考）

> **防止权限“被升级”或“被误给”**

即使：

* 管理员给了过大的 Identity Policy
* 或脚本误配置

👉 **Boundary 依然能兜底**

---

## 🚨 考试高频陷阱（你现在可以秒杀）

### ❌ 错误理解

> “Identity Policy 允许了，为什么还是不行？”

### ✅ 正确理解

> **Boundary 是权限天花板，不是权限补充**

---

## 🧠 SAA-C03 秒选口诀（直接背）

```text
• Boundary = 最大权限上限
• 最终权限 = Identity ∩ Boundary
• Boundary 不授予新权限
• Identity 多给的权限 = 作废
• Deny 或 Boundary 缺失 = 拒绝
```

---

## 🧪 一道考试题你现在能秒杀

**题目：**

> 用户有 S3 FullAccess，但仍无法创建 bucket，最可能的原因？

✅ **Permissions Boundary 限制了 CreateBucket**

---

如果你愿意，下一步我可以帮你：

1️⃣ 把 **Identity vs Boundary vs SCP** 画成一张对比速记表
2️⃣ 出 **5 道 Permissions Boundary 陷阱选择题（真考风）**
3️⃣ 把这套内容压成 **30 秒考前背诵卡**

你只要回一个数字即可。
