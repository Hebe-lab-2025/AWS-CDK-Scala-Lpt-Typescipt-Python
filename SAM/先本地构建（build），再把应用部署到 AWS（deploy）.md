这条命令是 **SAM 最标准的一步到位流程**。我按**“逐段解释 + 实际发生了什么 + 常见坑”**给你拆清楚。

---

# 一、整条命令在干嘛（一句话）

> **先本地构建（build），再把应用部署到 AWS（deploy）**
> 本质：**CloudFormation 创建/更新一个 stack**

---

# 二、`sam build` 在做什么？

```bash
sam build
```

### 它做的 3 件事

1. **读取 `template.yaml`**
2. **收集 Lambda 代码（CodeUri）**
3. **生成可部署产物**

### 产物位置

* 构建输出目录：

  ```
  .aws-sam/build/
  ```
* 构建后的模板：

  ```
  .aws-sam/build/template.yaml
  ```

### 关键点

* 不创建任何 AWS 资源
* 纯本地操作
* 把 **SAM 语法糖展开成 CloudFormation**

---

# 三、`sam deploy` 在做什么？

```bash
sam deploy \
  --stack-name clab-stack \
  --s3-bucket sam-clab-bucket-097285037130 \
  --capabilities CAPABILITY_IAM
```

这是 **真正“上云”的一步**。

---

## 1️⃣ `--stack-name clab-stack`

* CloudFormation Stack 名
* 行为规则：

  * 已存在 → **Update**
  * 不存在 → **Create**

👉 你现在的所有资源都“挂”在这个 stack 下。

---

## 2️⃣ `--s3-bucket sam-clab-bucket-097285037130`

### 为什么必须要 S3？

* Lambda 代码 + 打包模板 **必须先上传 S3**
* CloudFormation 再从 S3 拉取

### 这一步实际发生：

* `.aws-sam/build` 内容被 zip
* 上传到这个 bucket
* 生成一个带 S3 URL 的模板

⚠️ 这个 bucket **必须已存在**

---

## 3️⃣ `--capabilities CAPABILITY_IAM`

### 为什么要这个？

因为你的模板里 **涉及 IAM 相关资源**：

* Lambda 执行角色
* 或 SAM 自动生成的 IAM Role / Policy

CloudFormation 出于安全考虑：

> **你必须显式同意“我允许创建/修改 IAM 资源”**

不加这个参数 → **直接失败**

---

# 四、这条命令执行时的完整幕后流程（很重要）

```
sam build
  ↓
生成 .aws-sam/build/template.yaml
  ↓
sam deploy
  ↓
上传代码到 S3
  ↓
创建 CloudFormation ChangeSet
  ↓
执行 ChangeSet
  ↓
创建 / 更新资源：
  - DynamoDB
  - Lambda
  - IAM Role / Policy
  - （如果有）API Gateway
```

---

# 五、为什么不用 `sam deploy --guided`？

你这条命令是 **“非交互式部署”**：

* 所有参数都写死在命令里
* 适合：

  * lab
  * CI/CD
  * 重复部署

`--guided` 只适合第一次手把手配置。

---

# 六、最常见 5 个坑（你已经避开了）

1. ❌ 忘记 `sam build`
2. ❌ S3 bucket 不存在
3. ❌ 忘记 `--capabilities CAPABILITY_IAM`
4. ❌ Stack name 拼错导致创建新 stack
5. ❌ CodeUri / Handler 和文件结构不匹配

---

# 七、超短背诵版（面试 / 考试）

> `sam build` 负责本地打包，
> `sam deploy` 负责上传到 S3 并通过 CloudFormation 创建资源，
> `--capabilities CAPABILITY_IAM` 用于授权 IAM 资源创建。

---

如果你愿意，下一步我可以帮你：

* 🔍 对照你当前 **`.aws-sam/build/template.yaml`** 逐行解释
* 🧪 出 5 道 **SAM build / deploy 易错题**
* 🧠 画一张 **SAM → CloudFormation → AWS 资源**的执行图
