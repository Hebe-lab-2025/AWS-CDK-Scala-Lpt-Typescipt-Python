## 1️⃣ 用 CLI 创建一个 S3 bucket 并上传文件（可直接照抄）

> 前提：你已经 `aws configure` 配好 **Access Key / Secret / Region**（或用 SSO/Role）。

### A) 先选变量（避免手打出错）

```bash
# 1) 你的默认 region（建议跟你 aws configure 一致）
REGION="us-west-2"

# 2) bucket 名必须全局唯一：全小写 + 数字 + 连字符
BUCKET="web-content-bucket-$(date +%s)"

# 3) 你要上传的文件路径（举例）
FILE="./index.html"
```

### B) 创建 bucket（us-east-1 和非 us-east-1 写法不同）

```bash
# 如果 REGION 不是 us-east-1（推荐用这个）
aws s3api create-bucket \
  --bucket "$BUCKET" \
  --region "$REGION" \
  --create-bucket-configuration LocationConstraint="$REGION"

# 如果 REGION 是 us-east-1（特殊：不能带 create-bucket-configuration）
# aws s3api create-bucket --bucket "$BUCKET" --region us-east-1
```

### C) 上传文件（最常用）

```bash
aws s3 cp "$FILE" "s3://$BUCKET/"
```

### D) 验证（列出 / 查看对象）

```bash
aws s3 ls "s3://$BUCKET/"
aws s3api head-object --bucket "$BUCKET" --key "$(basename "$FILE")"
```

---

## 2️⃣ 用 CLI 启动/查看 EC2（只读权限版）

### A) “只读权限版”能做什么？

你**不能** `run-instances / start-instances / stop-instances`，但你可以：

* 查看实例、状态、AMI、类型、Security Group、VPC/Subnet 等

### B) 最常用的只读查看命令

```bash
# 1) 列出所有实例（精简输出）
aws ec2 describe-instances \
  --query "Reservations[].Instances[].{ID:InstanceId,State:State.Name,Type:InstanceType,AZ:Placement.AvailabilityZone,PublicIP:PublicIpAddress,Name:Tags[?Key=='Name']|[0].Value}" \
  --output table

# 2) 只看“实例状态检查”（考试高频）
aws ec2 describe-instance-status \
  --include-all-instances \
  --query "InstanceStatuses[].{ID:InstanceId,State:InstanceState.Name,System:SystemStatus.Status,Instance:InstanceStatus.Status}" \
  --output table

# 3) 查某个实例的详细信息（把 i-xxxx 换成你的）
aws ec2 describe-instances --instance-ids i-xxxxxxxxxxxxxxxxx
```

### C) 你想“模拟启动/停止”，但权限只读怎么办？

你会看到类似：

* `UnauthorizedOperation`
* `You are not authorized to perform: ec2:StartInstances`

这就是“只读权限版”的预期结果（考试也爱考）。

### D) 只读 IAM Policy（给你一份最小可用）

> 这段**只允许 describe***（查看），不能启动/停止/创建。

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EC2ReadOnly",
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 3️⃣ 模拟考试题：**“这条命令为什么失败？”**（排错训练 10 题）

下面每题都按“命令 → 报错现象 → 秒杀原因 → 一句话修法”。

### 题 1：创建 bucket 失败（最经典）

```bash
aws s3api create-bucket --bucket my-test-bucket --region us-west-2
```

* **失败原因**：缺少 `--create-bucket-configuration LocationConstraint=...`
* **修法**：加上 LocationConstraint（除非 region 是 us-east-1）

---

### 题 2：us-east-1 创建 bucket 反而失败

```bash
aws s3api create-bucket --bucket my-test-bucket --region us-east-1 \
  --create-bucket-configuration LocationConstraint=us-east-1
```

* **失败原因**：**us-east-1 不能带 LocationConstraint**
* **修法**：去掉 `--create-bucket-configuration`

---

### 题 3：bucket 名不合规

```bash
aws s3api create-bucket --bucket Web_Content_Bucket --region us-west-2 ...
```

* **失败原因**：bucket 名必须 **全小写**，不能有下划线/大写
* **修法**：改成 `web-content-bucket-123`

---

### 题 4：bucket 名“看起来对”但还是失败

```bash
aws s3api create-bucket --bucket web-content-bucket --region us-west-2 ...
```

* **失败原因**：bucket 名 **全局唯一**，已被别人占用
* **修法**：加随机后缀（时间戳/uuid）

---

### 题 5：上传失败（NoSuchBucket）

```bash
aws s3 cp index.html s3://my-bucket/
```

* **失败原因**：bucket 不存在 / 拼错 / 在另一个账号/region
* **修法**：`aws s3 ls` 看 bucket 是否存在；确认账号与 region

---

### 题 6：上传失败（AccessDenied）

```bash
aws s3 cp index.html s3://my-bucket/
```

* **失败原因**：没权限 `s3:PutObject`，或 bucket policy 拒绝
* **修法**：给 IAM 加 `s3:PutObject` + `s3:ListBucket`，并检查 bucket policy / Block Public Access

---

### 题 7：EC2 启动失败（只读权限）

```bash
aws ec2 start-instances --instance-ids i-123
```

* **失败原因**：缺少 `ec2:StartInstances` 权限（只读账号必考）
* **修法**：要么换有权限的 role，要么只做 `describe-*`

---

### 题 8：describe 也失败（AuthFailure / InvalidClientTokenId）

```bash
aws ec2 describe-instances
```

* **失败原因**：凭证无效/过期/环境变量覆盖了正确凭证
* **修法**：`aws sts get-caller-identity` 验证当前身份；检查 `AWS_ACCESS_KEY_ID` 等环境变量

---

### 题 9：Region/AZ 写错导致各种奇怪错误

```bash
aws ec2 describe-instances --region us-west-9
```

* **失败原因**：region 不存在
* **修法**：`aws ec2 describe-regions --output table` 查可用 region

---

### 题 10：命令成功但查不到你要的实例（“空输出”陷阱）

```bash
aws ec2 describe-instances --filters "Name=tag:Name,Values=MyServer"
```

* **失败原因**：Tag 名/值不匹配，或实例在别的 region
* **修法**：去掉 filter 先全量查；然后确认 `--region` 一致

---

如果你愿意，我可以把第 3 部分升级成**真考模式**：
你发我你最近 5 条失败的 CLI 命令（原样粘贴），我按 **“错误 → 根因 → 1 行修复命令 → 考试秒杀关键词”** 给你做一套专属错题本。
