下面我按 **“这份 policy 允许什么 / 拒绝什么 / 默认 Deny 是啥”** 来讲清楚（你提到的 **3️⃣ 默认 Deny** 我会重点解释）。

---

## 0) 先给一句话总结

这份策略是一个“**最小权限 + 强约束**”的组合：

* ✅ 只允许你做 **S3（指定 bucket 前缀）+ IAM（创建/绑定指定角色）+ Lambda（指定函数名）+ EC2（但只能很小规格）**
* ❌ 其它没写到的操作，一律不行（这就是 **默认 Deny**）
* ❌ 还额外写了 **显式 Deny**：限制 Region、EC2 机型、磁盘类型、磁盘大小（Deny 优先级最高）

---

## 1) 你问的重点：什么是 “默认 Deny”？

**默认 Deny（Implicit Deny）** = IAM 的“默认规则”：

> 如果某个操作没有被任何 Allow 明确允许，那它就 **默认不允许**。

所以哪怕你没有看到 “Deny” 字样，只要没写 Allow，它也是 **不允许**。

举例：

* 你 policy 里允许 `s3:PutObject` 到 `web-content-bucket*/*`
* 但你没允许 `s3:PutBucketLogging`
  👉 那就是 **默认 Deny**，会被拒绝（不是因为显式 Deny，而是因为没给权限）

⚠️ 对比：

* **默认 Deny**：没写 Allow → 拒绝
* **Explicit Deny**：写了 Deny → 更强，哪怕别处 Allow 也救不了

---

## 2) 这份策略逐块在干嘛（按模块）

### A) S3：只能操作名字匹配 `web-content-bucket*` 的桶和对象

**1）列出所有桶（账户级）**

```json
"s3:ListAllMyBuckets"
```

✅ 允许你在控制台看到 bucket 列表（不代表能进每个桶）

**2）管理 bucket（只限 `arn:aws:s3:::web-content-bucket*`）**
允许：

* Create/Delete bucket
* Public access block（强制禁止公开）
* Get/Put BucketPolicy
* Put encryption
* ListBucket / ListVersions
* `s3:Get*`（所有 Get 开头的 bucket/对象相关读取动作，但依然受 Resource 限制）

**3）管理对象（只限桶里的对象 `.../*`）**
允许：

* Put/Get/Delete object
* DeleteObjectVersion

✅ 结论：你只能把网页文件上传到指定前缀的 bucket 里，做静态站点/内容托管那种用途。

---

### B) IAM：只让你创建/管理两种固定角色，并且严格限制 PassRole

你能：

* `iam:ListRoles`, `iam:ListPolicies`（只能列举）
* 对 `role/LambdaRole` 做 create/get/update assume policy 等
* 对 `role/AccessBucketRole` 做类似操作
* 对 instance profile `instance-profile/AccessBucketRole` 做 create/delete/addRole/removeRole

✅ 重点安全点：**PassRole 被条件限制**

* `iam:PassRole` 给 Lambda 时只能 `iam:PassedToService = lambda.amazonaws.com`
* `iam:PassRole` 给 EC2 时只能 `iam:PassedToService = ec2.amazonaws.com`

👉 这避免你把角色乱塞给别的服务（比如 ECS、Glue 等）。

---

### C) Attach/Detach policy：只能绑一个叫 `S3AccessPolicy` 的 policy

这段：

```json
"Condition": { "ArnEquals": { "iam:PolicyARN": ["...:policy/S3AccessPolicy"]}}
"Action": ["iam:AttachRolePolicy","iam:DetachRolePolicy"]
```

✅ 意思：你只能给 `AccessBucketRole` / `LambdaRole` 绑定（或解绑）那一个指定的 `S3AccessPolicy`，不能随便绑别的更大权限策略。

---

### D) Lambda：只能管一个固定名字的函数 `WebPageFunction`（且仅 us-east-1）

允许：

* ListFunctions / GetAccountSettings（只读列表）
* 对指定 ARN 的函数：Create/Update/Delete/Invoke/CreateFunctionUrlConfig 等

✅ 用途：创建一个 Lambda + Function URL 作为网页后端或 API 入口。

---

### E) EC2：能创建/管理，但被 3 个 Deny 强约束

Allow 里给了很多 EC2 权限（RunInstances、SG、Describe、Start/Stop、AssociateIamInstanceProfile 等）

但下面 3 个 Deny 会“卡死”你：

**D1：实例类型只能 t2.micro / t3.micro**

```json
Deny ec2:RunInstances if InstanceType NOT LIKE t2.micro or t3.micro
```

**D2：磁盘类型只能 gp2/gp3**

```json
Deny ec2:RunInstances if VolumeType NOT LIKE gp2 or gp3
```

**D3：磁盘大小不能大于 8GB**

```json
Deny ec2:RunInstances if VolumeSize > 8
```

👉 结论：你只能起“很省钱的最小 EC2”。

---

## 3) 最强的一条：RegionPolicy（显式 Deny 全部服务）

```json
"Effect": "Deny",
"Action": "*",
"Condition": { "StringNotEquals": { "aws:RequestedRegion": "us-east-1" } }
```

这条的效果是：

> 只要你请求的 region 不是 us-east-1，**所有 AWS API 直接拒绝**（无论你上面 Allow 了什么）。

所以你在 CLI 必须这样：

```bash
aws --region us-east-1 ...
```

或者配置默认 region 为 us-east-1。

---

## 4) 你这份策略的“使用方式”像什么？

像一个教学/实验账号的“沙盒权限”：

* 只能在 **us-east-1**
* 只能建最便宜 EC2
* 只能操作特定命名规则的 S3 bucket
* 只能创建指定名字的 Lambda/角色
* 通过角色给 EC2/Lambda 访问 S3（而不是直接给用户大权限）

---

## 5) 你最容易踩的坑（和你刚才 CLI 的关系）

1. 你如果在控制台切到 `us-west-2`：**全部报 AccessDenied**（因为 RegionPolicy）
2. 你如果 RunInstances 选了 `t3.small`：**即使 Allow 了 RunInstances 也会失败**（D1）
3. 你如果想做别的服务（比如 RDS、CloudFront、DynamoDB）：多数会失败（**默认 Deny**，因为没给 Allow）

---

如果你愿意，我可以把这份 policy **再压缩成“暗号速记版”**（考试/面试那种：看到 Sid 就知道在控什么），或者你说你现在要做的目标（比如：用 EC2 + S3 部署静态网页 / 用 Lambda URL 读取 S3），我帮你对照这份权限一步步走，不踩 Deny。
