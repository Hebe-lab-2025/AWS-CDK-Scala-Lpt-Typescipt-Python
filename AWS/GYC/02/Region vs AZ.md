好，我们**一步一步来，像真人带你做 Lab 一样**。
你可以一边看一边在终端敲，我会告诉你**每一步该看到什么**。

---

## 1️⃣ 完成 `aws configure`（实时核对版）

### Step 1：执行命令

```bash
aws configure
```

你会看到 4 行提示，**逐行来**👇

---

### Step 2：输入 Access Key

```text
AWS Access Key ID [None]: AKIAxxxxxxxxxxxx
```

✅ 核对点：

* 以 **AKIA** 开头（或 ASIA，代表临时凭证）
* 中间没有空格

---

### Step 3：输入 Secret Key

```text
AWS Secret Access Key [None]: xxxxxxxxxxxxxxxxxxxxx
```

⚠️ 注意：

* **输入时不会显示**
* 粘贴也看不到，属于正常

---

### Step 4：输入默认 Region（非常重要）

```text
Default region name [None]: us-east-1
```

📌 建议你现在用：

* `us-east-1`（考试 / Lab / S3 网站最常见）

---

### Step 5：输出格式

```text
Default output format [None]: json
```

---

### Step 6：验证是否成功

```bash
aws sts get-caller-identity
```

**正确结果长这样：**

```json
{
  "UserId": "...",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/xxx"
}
```

❌ 如果报错：

* `Unable to locate credentials` → configure 没成功
* `InvalidClientTokenId` → key 错 / 过期

---

## 2️⃣ 怎么 **临时切换 Region**（3 种方式，考试+实战）

### ✅ 方法 1（最常用）：命令行参数

```bash
aws s3 ls --region us-west-2
```

📌 **只影响这一条命令**

---

### ✅ 方法 2：环境变量（当前终端会话）

```bash
export AWS_DEFAULT_REGION=us-west-2
```

验证：

```bash
aws configure get region
```

---

### ✅ 方法 3：修改默认配置

```bash
aws configure
```

重新输入 region 即可（**全局生效**）

---

## 3️⃣ Region vs AZ（可用区）——一句话就够

### 🌍 Region（区域）

* 一个 **地理区域**
* 例子：

  * `us-east-1`（北弗吉尼亚）
  * `us-west-2`（俄勒冈）
* **资源隔离边界**
* S3 bucket / Lambda / IAM 都“属于某个 Region”

---

### 🏢 AZ（Availability Zone）

* **Region 内的数据中心**
* 例子：

  * `us-east-1a`
  * `us-east-1b`
* **物理隔离**
* 用来做 **高可用**

---

### 🧠 考试秒杀版

```text
Region = 城市
AZ = 城市里的不同大楼
```

📌 高频考点：

* **Multi-AZ = 高可用**
* **跨 Region = 灾备（DR）**
* AZ 名字在不同账号里可能不一样（1a ≠ 物理同一个）

---

## 4️⃣ 跑一条 **真正的 AWS 命令**（你现在就能验证）

### ✅ S3 命令（最安全）

```bash
aws s3 ls
```

看到：

```text
2024-01-01  my-bucket-1
2024-01-02  my-bucket-2
```

说明：

* ✅ 凭证 OK
* ✅ Region OK
* ✅ S3 权限 OK

---

### ⚠️ EC2 命令（可能需要权限）

```bash
aws ec2 describe-instances --region us-east-1
```

* 如果有实例 → 会返回 JSON
* 如果没权限 → 会看到 `UnauthorizedOperation`
  👉 **这是正常的**（说明你没 EC2 权限）

---

## 5️⃣ 你现在的状态自检（3 秒）

如果你能做到：

* `aws sts get-caller-identity` ✅
* `aws s3 ls` ✅

👉 **CLI 环境 100% 配好，可以继续所有 Lab / 考试 / 实战**

---

### 下一步我可以继续带你（选一个）：

1️⃣ 用 CLI **创建一个 S3 bucket 并上传文件**
2️⃣ 用 CLI **启动/查看 EC2（只读权限版）**
3️⃣ 模拟考试题：**“这条命令为什么失败？”排错训练**

你直接回 **1 / 2 / 3**，我接着带你敲。



