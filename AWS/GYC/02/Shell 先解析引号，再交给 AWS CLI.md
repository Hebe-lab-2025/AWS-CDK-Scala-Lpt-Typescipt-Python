你这条命令**逻辑是对的**，但 **Shell 被引号骗了**。我直接给你**结论 + 正确写法 + 心智模型**。

---

## ❌ 为什么你这条会出问题？

```bash
aws ec2 describe-availability-zones --query "AvailabilityZones[*].{"RegionName":RegionName,"ZoneName":ZoneName}" --output table
```

问题只有一个：
👉 **双引号嵌套双引号，Shell 解析炸了**

Shell 会这样理解：

```text
"AvailabilityZones[*].{   ← 这里结束了
"RegionName"              ← 新字符串
```

所以不是 AWS CLI 的错，是 **zsh/bash 语法层面的问题**。

---

## ✅ 正确写法（3 选 1，任选）

### ✅ 写法 1（最推荐，最干净）

**外层用单引号**

```bash
aws ec2 describe-availability-zones \
  --query 'AvailabilityZones[*].{RegionName:RegionName,ZoneName:ZoneName}' \
  --output table
```

📌 **CLI 老手默认写法**

---

### ✅ 写法 2（你原本想写的，但要转义）

**转义内部双引号**

```bash
aws ec2 describe-availability-zones \
  --query "AvailabilityZones[*].{\"RegionName\":RegionName,\"ZoneName\":ZoneName}" \
  --output table
```

📌 可用，但不优雅

---

### ✅ 写法 3（不用 map，最简单）

```bash
aws ec2 describe-availability-zones \
  --query 'AvailabilityZones[*].[RegionName,ZoneName]' \
  --output table
```

输出一样，语法更短。

---

## 🧠 一次记住的心智模型（非常重要）

### 规则 1️⃣

**Shell 先解析引号，再交给 AWS CLI**

不是 AWS 在解析这一步，是 **zsh/bash**

---

### 规则 2️⃣

> **JMESPath 里需要 `{}` 时，外层一定用单引号**

这是 AWS CLI 的“潜规则”

---

## 🧪 你跑成功后，正确输出应该是

```
---------------------------------
| DescribeAvailabilityZones     |
+-------------+-----------------+
| RegionName  | ZoneName        |
+-------------+-----------------+
| us-east-1   | us-east-1a      |
| us-east-1   | us-east-1b      |
| us-east-1   | us-east-1c      |
| us-east-1   | us-east-1d      |
| us-east-1   | us-east-1e      |
| us-east-1   | us-east-1f      |
+-------------+-----------------+
```

---

## 🎯 一句话总结（面试 / 实战都适用）

> AWS CLI 的 `--query` 是 JMESPath 表达式，包含 `{}` 时应使用单引号包裹以避免 Shell 引号冲突。

---

你现在已经在 **CLI → JMESPath → Infra 可视化** 这条正确路径上了。

如果你愿意，下一步我可以直接带你做其中一个：

* 🔥 `describe-subnets`（把 AZ 变成真实网络）
* 🔥 `describe-instances`（把 EC2 真正“落地”）
* 🔥 用 **一条命令** 看懂「EC2 在哪个 AZ」

你点一个，我继续带。
