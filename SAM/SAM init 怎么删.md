下面直接给你 **「SAM init 怎么删 / SAM 项目怎么清理」的标准做法**，按**使用场景**分清楚（很像考试问法）。

---

## 一、先给结论（30 秒记住）

> **`sam init` 本身没有 delete 命令**
> 你要删的是：
>
> * **本地生成的文件**（代码）
> * **云端部署的资源**（CloudFormation Stack）

---

## 二、情况 1️⃣：只想删 `sam init` 生成的**本地项目**（最常见）

### `sam init` 实际做了什么？

👉 只是：

* 在当前目录 **生成一堆文件**
* **没有创建任何 AWS 资源**

### 删除方式（最简单）

```bash
rm -rf <sam-project-folder>
```

例如：

```bash
rm -rf sam-app
```

✅ 这就等价于 “delete sam init”

---

## 三、情况 2️⃣：已经 `sam deploy`，想删**云端资源**（重点）

### ❌ 错误理解（很多人会犯）

```bash
sam delete init   ❌（不存在）
```

---

### ✅ 正确方式：`sam delete`

```bash
sam delete --stack-name clab-stack
```

### 这条命令会做什么？

* 删除 **CloudFormation Stack**
* 自动清理：

  * Lambda
  * API Gateway
  * IAM Role（如果是 stack 创建的）
* **不会删除 S3 bucket（默认）**

---

## 四、完整、安全的删除流程（推荐）

### 1️⃣ 删除云端资源

```bash
sam delete --stack-name clab-stack
```

👉 出现提示时输入：

```
y
```

---

### 2️⃣ 删除本地项目

```bash
rm -rf sam-app
```

---

## 五、常见追问 / 易错点（面试爱问）

### Q1：`sam delete` 会不会删 S3 bucket？

**默认不会**

* 只删除 stack 管理的资源
* bucket 一般是 **外部资源**

---

### Q2：为什么 SAM 不自动删 bucket？

因为：

* S3 bucket **可能被多个项目共用**
* AWS 避免误删数据（安全设计）

---

### Q3：如果我真想删 S3 bucket？

```bash
aws s3 rb s3://<bucket-name> --force
```

⚠️ 会 **清空 bucket + 删除 bucket**

---

## 六、一句话记忆版（考试 / 面试）

> `sam init` 只生成本地文件，删除就是删目录；
> `sam delete` 删除的是 CloudFormation Stack 和云端资源。

---

## 七、给你一张 mental model（脑图）

```
sam init
  ↓
[ 本地文件 ]
  ↓ sam deploy
[ CloudFormation Stack ]
  ├─ Lambda
  ├─ API Gateway
  └─ IAM Role

删除逻辑：
- 本地 → rm -rf
- 云端 → sam delete
```

如果你愿意，我可以下一步帮你：

* 🧪 出 5 道 **SAM / CloudFormation 清理陷阱题**
* 🧠 用一句口诀区分 **sam delete vs aws cloudformation delete-stack**
* 🔍 模拟一次 **“为什么 sam delete 失败” 的排错流程**
