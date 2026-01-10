下面按你 3 个点直接给到可用答案（偏 SDE/面试口径，能直接 copy）。

---

## 1) ✅ 给 **EC2 Role** 用的 **最小权限 S3 策略**（常见：只读某个 bucket 的某个前缀）

假设：

* Bucket：`my-app-bucket`
* 只允许读：`public/` 这个目录下的对象（包含子目录）

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListOnlyThisPrefix",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-app-bucket",
      "Condition": {
        "StringLike": {
          "s3:prefix": ["public/*"]
        }
      }
    },
    {
      "Sid": "ReadObjectsInPrefix",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::my-app-bucket/public/*"
    }
  ]
}
```

### 你可能会用到的变体

**A. 只读整个 bucket 所有对象（更宽）**

* `ListBucket` 资源还是 bucket ARN
* `GetObject` 资源是 `bucket/*`

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListBucket",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-app-bucket"
    },
    {
      "Sid": "GetAllObjects",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-app-bucket/*"
    }
  ]
}
```

**B. 只写入某个前缀（上传）**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PutOnlyToUploadsPrefix",
      "Effect": "Allow",
      "Action": ["s3:PutObject"],
      "Resource": "arn:aws:s3:::my-app-bucket/uploads/*"
    }
  ]
}
```

> 最小权限口诀：
> **ListBucket = 看“目录/键名列表”**（bucket ARN）
> **GetObject/PutObject = 操作“文件对象”**（object ARN）

---

## 2) 🔍 为什么 **ListBucket** 和 **GetObject** 必须分开授权？

### 原因 1：它们操作的是 **不同资源类型**

* `s3:ListBucket` → 作用对象是 **Bucket**
  ✅ 资源必须是：`arn:aws:s3:::bucket-name`
* `s3:GetObject` → 作用对象是 **Object**
  ✅ 资源必须是：`arn:aws:s3:::bucket-name/*`

所以你不能用一个 Resource ARN 同时覆盖两者（权限模型本身就分层）。

---

### 原因 2：安全边界不同（“列目录”本身就是泄露）

* 有 `ListBucket`：攻击者可以枚举对象 key（知道你有哪些文件、路径结构、命名规律）
* 没有 `ListBucket`：攻击者 **猜不到 key** 就很难拿到对象（即使有 GetObject 也需要知道 key）

✅ 最小权限的最佳实践通常是：

* **只给 GetObject（知道 key 才能取）**
* 如果业务确实要“浏览/分页列出”，再给 `ListBucket`，并用 `s3:prefix` 限定范围

---

### 原因 3：产品功能上就常常分开用

* 网站/APP “展示列表” → 需要 ListBucket
* 具体下载一个文件 → 需要 GetObject

这也是为什么你经常看到两个 Statement。

---

## 3) 🆚 S3 vs AliOSS vs MinIO vs Client（SDE 视角对比）

### A) “权限模型 / 授权方式”对比（最关键）

| 组件                 | 身份与权限                                 | 最常见做法                                  |
| ------------------ | ------------------------------------- | -------------------------------------- |
| **AWS S3**         | IAM Role/Policy + Bucket Policy       | EC2 用 **Instance Profile(Role)** 拿临时凭证 |
| **AliOSS**         | RAM Role/Policy                       | ECS 用 **RAM Role** 或 STS 临时 token      |
| **MinIO**          | Access Key/Secret + Policy（兼容 S3 API） | 通常是服务端保管 AK/SK 或用 STS（看部署）             |
| **Client（前端/移动端）** | 不应持久保存密钥                              | 用 **STS 临时凭证** 或 **预签名 URL**           |

**关键结论：**

* **生产环境别把长期 AK/SK 放在客户端**
* 前端/移动端最佳：**Pre-signed URL** 或 **STS 临时 token**
* EC2/ECS 最佳：**Role（Instance Profile / RAM Role）** 自动轮换临时凭证

---

### B) “S3 和 MinIO 为什么经常放一起说？”

因为 **MinIO 兼容 S3 API**：

* SDK 接口几乎一样
* 迁移成本低（很多项目可切 endpoint）

差异主要在：

* S3：托管、无限扩展、强生态（事件/生命周期/跨区复制）
* MinIO：自建/私有化/可控，但你要负责运维、容量、HA、升级

---

### C) Client 访问对象存储的两种安全姿势

**1）Pre-signed URL（推荐下载/上传）**

* 服务器签一个短期 URL（比如 5 分钟）
* 客户端直接 PUT/GET 到对象存储
* 服务端不需要转发大文件流量（省钱+快）

**2）STS 临时凭证（更灵活）**

* 客户端先向你服务端申请临时 token
* 客户端拿 token 调对象存储 API
* 好处：能做更复杂的权限与目录隔离

---

如果你把你现在那段 S3 Bucket Policy / Role Policy 原文贴一下（或告诉我你是“要列出目录”还是“只下载固定文件”），我可以把它**精确收敛成最小权限**版本（包含 prefix、可选 KMS、可选 CloudFront/OAI 的版本）。
