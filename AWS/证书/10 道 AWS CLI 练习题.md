好呀，那我就按你说的这三块来，一次性帮你搞定👇

---

## 一、10 道 AWS CLI 练习题（偏概念 + 实战）

**Q1.** 想在本机第一次使用 AWS CLI，需要先运行哪条命令来完成基础配置？
A. `aws init`
B. `aws setup`
C. `aws configure`
D. 不需要配置，直接用

> ✅ 正确：C。`aws configure` 用来输入 **Access Key / Secret Access Key / region / output format**。

---

**Q2.** 运行 `aws configure` 时，下面哪一项**不是必填项**？
A. AWS Access Key ID
B. AWS Secret Access Key
C. Default region name
D. Default output format

> ✅ 正确：D。输出格式不填有默认值，一般是 `json`。

---

**Q3.** 下面哪项**不能**直接通过 `aws configure` 设置？
A. 默认区域（region）
B. 默认输出格式（output format）
C. 默认 IAM Role 名称
D. Access Key / Secret Key

> ✅ 正确：C。role 通常通过 profile + `source_profile` / `role_arn` 等方式配置在 `~/.aws/config`，不是 `aws configure` 的交互项。

---

**Q4.** 你想在命令行列出当前账号下的 S3 bucket，应该用哪条命令？
A. `aws s3 ls`
B. `aws s3 cp`
C. `aws ec2 describe-buckets`
D. `aws s3api list-objects`

> ✅ 正确：A。
> `aws s3 ls` 是 S3 高级命令列出 bucket 列表。

---

**Q5.** 你有两个 profile：`default` 和 `dev`，想用 `dev` 这个 profile 列出 S3 bucket，用哪条命令？
A. `aws s3 ls --profile dev`
B. `aws configure dev`
C. `aws dev s3 ls`
D. `aws s3 ls --use-dev`

> ✅ 正确：A。`--profile` 用来选择不同配置。

---

**Q6.** 想以更底层 API 形式列出 S3 bucket（不是 s3 高级命令），应该用：
A. `aws s3 list-buckets`
B. `aws s3api list-buckets`
C. `aws ec2 describe-buckets`
D. `aws s3 buckets`

> ✅ 正确：B。`s3api` 是“接近底层 API 名字”的子命令。

---

**Q7.** 以下关于 AWS CLI 的说法，哪一项是**正确的**？
A. 只能在 Linux 上使用
B. 每次使用 CLI 都必须重新输入 Access Key
C. 可以通过 `~/.aws/credentials` 和 `~/.aws/config` 保存配置
D. CLI 直接使用 root 账号，不支持 IAM 用户

> ✅ 正确：C。CLI 推荐使用 **IAM 用户 / Role** + 本地配置文件。

---

**Q8.** 你在服务器上发现明文保存的 root Access Key 和 Secret Key，这种做法违反了哪条安全建议？
A. 最小权限原则（Principle of Least Privilege）
B. 使用多区域部署
C. 使用预留实例
D. 使用 CloudFormation

> ✅ 正确：A。root 权限过大，且明文保存非常危险。

---

**Q9.** 你希望通过 CLI 在 EC2 实例上执行操作，但不想在实例上保存任何 Access Key / Secret Key，应该使用哪种机制？
A. IAM Role 绑定给 EC2 实例
B. Root 账号登录 EC2
C. 把 Access Key 写进脚本
D. 用本地电脑的 CLI 远程控制

> ✅ 正确：A。给 EC2 绑定 **Instance Profile / IAM Role**，自动获得临时凭证。

---

**Q10.** 想把 CLI 默认输出从 JSON 改成 Table 格式，最快的方式是？
A. 重新安装 AWS CLI
B. 修改 `~/.aws/config` 或 `aws configure` 中的 `Default output format` 为 `table`
C. 必须在每条命令后加 `--output table`
D. 在 AWS 控制台里改设置

> ✅ 正确：B。可以全局改，也可以单次命令用 `--output` 覆盖。

---

## 二、一步步带你“在脑子里”跑一遍 `aws configure`

假设你本地已经安装好 AWS CLI v2（Mac / Windows / Linux 都类似）：

### 1️⃣ 准备前置条件（在控制台）

1. 登录 AWS 控制台（**不要用 root 长期干活，只用来创建 IAM 用户/Role**）。
2. 去 **IAM → Users → Create user**：

   * 比如叫：`cli-developer`
   * 附加策略（示例）：`AmazonS3ReadOnlyAccess`（最小权限）
3. 在该 IAM 用户页面 → **Create access key**：

   * 选择使用场景：Command Line Interface
   * 得到：**Access key ID** + **Secret access key**（只显示一次，要保存好）

> 真实考试思路：CLI 绝大多数情况使用 **IAM 用户 / Role 的 key**，不是 root key。

---

### 2️⃣ 在本机运行 `aws configure`

命令行输入：

```bash
aws configure
```

你会依次看到 4 个提示：

1. **AWS Access Key ID [None]:**
   👉 输入刚才 IAM 用户的 Access Key ID，例如：
   `AKIAIOSFODNN7EXAMPLE`

2. **AWS Secret Access Key [None]:**
   👉 粘贴 Secret Access Key，例如：
   `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

3. **Default region name [None]:**
   👉 比如你在美国西部(俄勒冈)：`us-west-2`
   （考试常见：`us-east-1`, `us-west-2`, `eu-west-1` 等）

4. **Default output format [None]:**
   👉 可以填：`json` / `table` / `text`，常见是 `json`。

完成后，本地会生成两个文件（Linux/Mac 默认在 `~/.aws/`）：

* `credentials`：保存 Access Key / Secret Key
* `config`：保存 region / output / profile 等配置

你可以用：

```bash
cat ~/.aws/credentials
cat ~/.aws/config
```

查看内容（注意：真实环境不要截图发给别人 🤭）。

---

### 3️⃣ 测试 CLI 是否正常

比如列出 S3：

```bash
aws s3 ls
```

如果能看到 Bucket 列表（或者为空但无报错），说明：

* 凭证正确
* 权限足够
* region / 网络也没问题

如果报错：

* `InvalidAccessKeyId` / `SignatureDoesNotMatch` → key 填错
* `AccessDenied` → IAM 权限不够
* `Unable to locate credentials` → 没有配置 / profile 名不对

---

## 三、IAM 安全最佳实践（面试/考试高频）

这里帮你整理成**背诵用要点**，你可以当成 “AWS 安全题的标准答案模板”。

### 1️⃣ 不要长期使用 root 账号 / root key

* root 权限 = 整个账号的“超管”，可以删账号、改付费方式，危险极高
* 最佳实践：

  * 给 root 打开 MFA
  * 只在必须场景用一次（例如改账单、创建第一个 IAM 用户）
  * **绝不为 root 创建长期 Access Key**（有的话要删掉）

> 面试说法：
> *“We avoid using the root user for day-to-day operations, and we never create long-term root access keys.”*

---

### 2️⃣ 使用 IAM 用户 / IAM Role + 最小权限原则 (Least Privilege)

* 为每类任务创建**单独的 IAM 用户或 Role**
* 只授予完成工作所需的最少权限，比如：

  * 只读 S3 → `AmazonS3ReadOnlyAccess`
  * 只操作特定 bucket → 自定义 policy 限定 `Resource`
* 定期审计权限，移除不再使用的权限。

> 面试句型：
> *“We follow the principle of least privilege by granting only the permissions required for a specific task or role.”*

---

### 3️⃣ 优先使用 **IAM Role + 临时凭证**，避免在服务器上放长期 key

* 在 EC2、Lambda、ECS 里面访问 AWS 资源时：
  👉 用 IAM Role 绑定给这些计算资源
* 好处：

  * 通过 STS 自动获取 **短期临时凭证**
  * 不需要在代码里写死 Access Key / Secret Key
  * 泄露风险更小，凭证自动轮换

> 关键点：
> *“For compute resources, we prefer IAM roles with temporary credentials instead of long-term access keys.”*

---

### 4️⃣ 开启多重验证（MFA）

* 对 root 用户、重要 IAM 用户开启 MFA：

  * 虚拟 MFA（Google Authenticator / Authy）
  * 硬件 MFA 设备
* 防止密码泄露后被直接登录。

---

### 5️⃣ 使用强密码策略 + 定期轮换 Access Key

* 配置 IAM Password Policy：

  * 最小长度、复杂度
  * 轮换周期
* Access Key：

  * 定期轮换
  * 删除不再使用的 key
  * 监控异常使用（配合 CloudTrail）

---

### 6️⃣ 审计和监控（CloudTrail / Config / CloudWatch）

* 打开 **CloudTrail** 记录管理平面 API 调用
* 用 **AWS Config** 跟踪资源配置变更
* 用 **CloudWatch / GuardDuty / Security Hub** 监控异常行为

> 考试记忆：
>
> * “谁做了什么？” → CloudTrail
> * “资源现在处于什么配置状态？” → Config

---

如果你愿意，下一步我可以帮你：

* 把这套内容整理成一页「**AWS CLI & IAM 安全 Cheat Sheet**」方便你考前快速复习；
* 或者出一套 **10 道 IAM / 安全场景题**（比如 root key 泄露怎么办、S3 公共访问怎么防止）帮你强化。
