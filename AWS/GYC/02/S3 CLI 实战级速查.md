下面给你一份 **S3 CLI 实战级速查 + 概念澄清 + 完整部署流程**（**考试能选、工作能用**）。

---

## ① S3 最常用 **AWS CLI** 命令速查表（高频）

> 前提：已配置好 `aws configure`（或使用 EC2 Role）

### 📦 Bucket 级（容器）

```bash
# 列出所有 bucket
aws s3 ls

# 创建 bucket（注意区域）
aws s3 mb s3://my-static-site-123 --region us-east-1

# 删除空 bucket
aws s3 rb s3://my-static-site-123

# 强制删除（含对象）
aws s3 rb s3://my-static-site-123 --force
```

### 📄 Object 级（文件）

```bash
# 列出 bucket 内对象
aws s3 ls s3://my-static-site-123/

# 上传单个文件
aws s3 cp index.html s3://my-static-site-123/index.html

# 下载文件
aws s3 cp s3://my-static-site-123/index.html ./index.html

# 删除对象
aws s3 rm s3://my-static-site-123/index.html
```

### 🔁 同步（最常用）

```bash
# 本地目录 → S3（部署静态站点必用）
aws s3 sync ./dist s3://my-static-site-123 --delete

# S3 → 本地
aws s3 sync s3://my-static-site-123 ./backup
```

### 🔓 权限 / 网站

```bash
# 关闭 Block Public Access（仅静态网站场景）
aws s3api put-public-access-block \
  --bucket my-static-site-123 \
  --public-access-block-configuration \
  BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false

# 设置静态网站托管
aws s3 website s3://my-static-site-123 \
  --index-document index.html \
  --error-document error.html
```

---

## ② **S3 Bucket vs Object**：概念一刀切清楚

### 🪣 Bucket（桶）

* **是什么**：对象的“容器”
* **全局唯一**：bucket 名字 **全 AWS 全球唯一**
* **权限层级**：

  * `s3:ListBucket`
  * `s3:PutBucketPolicy`
* **类比**：文件系统里的“顶级目录”

### 📄 Object（对象）

* **是什么**：文件本体（key + data + metadata）
* **存在于 bucket 内**
* **权限层级**：

  * `s3:GetObject`
  * `s3:PutObject`
* **类比**：目录里的“文件”

### 🔑 考试/面试秒杀点

* **ListBucket ≠ GetObject**（必须分开授权）
* **Bucket Policy** 作用于 bucket（可限制前缀）
* **Object ARN** 一定长这样：`arn:aws:s3:::bucket-name/path/*`

---

## ③ 用 CLI **部署静态网站到 S3**（完整流程）

> 目标：把本地静态页面部署成公网可访问网站
> 服务：**Amazon Web Services S3 静态网站托管**

### Step 0️⃣ 准备文件

```text
site/
 ├─ index.html
 ├─ error.html
 └─ assets/
```

---

### Step 1️⃣ 创建 Bucket（区域很重要）

```bash
aws s3 mb s3://my-static-site-123 --region us-east-1
```

> 📌 **考试点**：
>
> * 网站 endpoint 在 **us-east-1** 最常见
> * bucket 名必须全球唯一

---

### Step 2️⃣ 上传网站文件

```bash
aws s3 sync ./site s3://my-static-site-123
```

---

### Step 3️⃣ 允许公网访问（静态网站必须）

#### 3.1 关闭 Block Public Access

```bash
aws s3api put-public-access-block \
  --bucket my-static-site-123 \
  --public-access-block-configuration \
  BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false
```

#### 3.2 配置 Bucket Policy（**最小可用版**）

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-site-123/*"
    }
  ]
}
```

```bash
aws s3api put-bucket-policy \
  --bucket my-static-site-123 \
  --policy file://policy.json
```

> 📌 **考试秒杀**：
>
> * 静态网站 **不需要 ListBucket**
> * 只给 `GetObject` 就够

---

### Step 4️⃣ 启用静态网站托管

```bash
aws s3 website s3://my-static-site-123 \
  --index-document index.html \
  --error-document error.html
```

---

### Step 5️⃣ 访问网站

```text
http://my-static-site-123.s3-website-us-east-1.amazonaws.com
```

---

## 🎯 考试高频“反直觉”总结

* **S3 是 serverless**（很多人会忘）
* **静态网站 ≠ S3 REST endpoint**（要用 website endpoint）
* **Block Public Access 不关 → Policy 再对也访问不了**
* **不需要 EC2 / ALB / Lambda**

---

如果你愿意，我可以下一步帮你做三件事之一：
1️⃣ 把这套流程压成 **一页 CLI + 架构速记卡**
2️⃣ 出 **5 道 S3 静态网站部署陷阱题**（真考风）
3️⃣ 把它升级成 **S3 + CloudFront + OAC 的生产级方案**
