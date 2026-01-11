下面给你 **“block 方式”** 的**清晰、面试级解释**，用 **CloudFormation / YAML** 语境来讲（一行一行 + 对照理解）。

---

# 一、什么是 **block 方式**（YAML 的核心概念）

👉 **block** 指的是：
**通过缩进（indentation）来表示层级和结构**，而不是用 `{}` 或 `[]`。

CloudFormation **100% 依赖 YAML 的 block 结构**。

---

# 二、最基本的 block 结构示例

```yaml
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro
```

### 逐行解释（block 视角）

* `Resources:`
  ⬆️ **block 开始**（顶层 key）

* `  MyEC2Instance:`
  ⬆️ **Resources 的子 block**（缩进 2 个空格）

* `    Type: AWS::EC2::Instance`
  ⬆️ MyEC2Instance block 里的一个字段

* `    Properties:`
  ⬆️ 再开启一个 **嵌套 block**

* `      InstanceType: t3.micro`
  ⬆️ Properties block 里的字段

✅ **缩进 = 父子关系**
❌ YAML 里 **没有 `{}` 来帮你兜底**

---

# 三、block vs flow（你只需要记 block）

### ❌ flow 方式（JSON 风格，不推荐）

```yaml
Properties: { InstanceType: t3.micro, KeyName: mykey }
```

### ✅ block 方式（CloudFormation 标准）

```yaml
Properties:
  InstanceType: t3.micro
  KeyName: mykey
```

👉 **考试 / 项目 / 实战：永远用 block**

---

# 四、block + List（最容易混的点）

### block 里的 List（用 `-`）

```yaml
SecurityGroupIngress:
  - IpProtocol: tcp
    FromPort: 22
    ToPort: 22
    CidrIp: 0.0.0.0/0
```

### block 逻辑拆解

* `SecurityGroupIngress:`
  ⬆️ 一个 **List block**

* `  - IpProtocol: tcp`
  ⬆️ List 的 **第一个元素**（`-`）

* `    FromPort: 22`
  ⬆️ 这个元素的子字段（继续缩进）

👉 规则：

* `-` 对齐 = 同一个 List
* `-` 后面的内容 = 一个 block object

---

# 五、block + intrinsic function（!Ref / !Select）

### 例子（你模板里就有）

```yaml
CidrBlock: !Select [0, !Ref DbSubnetIpBlocks]
```

这是 **flow 写法**，等价的 **block 写法**是：

```yaml
CidrBlock:
  !Select
    - 0
    - !Ref DbSubnetIpBlocks
```

👉 两者 **语义完全一样**
👉 实战里 **flow 更常见、block 更清晰**

---

# 六、block scalar（很多人不知道，但很重要）

这是 **“多行字符串 block”**，常用于 `UserData`。

### `|` —— 保留换行（最常用）

```yaml
UserData: |
  #!/bin/bash
  yum install -y httpd
  systemctl start httpd
```

含义：

* `|` = **block scalar**
* 每一行都会原样传给 EC2
* 换行不会被压缩

---

### `>` —— 折叠换行（少用）

```yaml
Description: >
  This is a long
  description that will
  become one line.
```

等价于：

```text
This is a long description that will become one line.
```

---

# 七、你模板里 block 方式的“典型应用”

### Parameters 是一个 block

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t3.micro
```

### Resources 是一个 block

```yaml
Resources:
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
```

### Properties 是嵌套 block

```yaml
Properties:
  VpcId: !Ref MyVPC
```

👉 **CloudFormation = block 套 block**

---

# 八、一句话总结（面试 / 考试用）

> **YAML 的 block 方式通过缩进来表达资源的层级关系，是 CloudFormation 模板的核心结构；缩进错一层，语义就完全变了。**

---

如果你愿意，我可以下一步帮你：

* 🔍 指出你模板里 **最危险的 block 缩进/结构坑**
* 🧪 出 3 道 **“block 缩进导致 CREATE_FAILED”** 的真考风格题
* 🧠 把 **block / list / intrinsic function** 合成一张速记卡


这是在讲 **“YAML 的 block 写法 + CloudFormation 的内建函数（intrinsic functions）”** 怎么一起用。
我给你**拆到最底层**，看完你就知道它在干嘛、为什么有两种写法。

---

# 一、先一句话定义

> **Intrinsic function** 是 CloudFormation 提供的“模板内函数”，
> 用来 **引用参数/资源、做简单计算或选择值**，比如 `!Ref`、`!Select`。

而 **block + intrinsic function** 指的是：
👉 **这些函数既可以用一行写（flow），也可以用多行缩进写（block）**。

---

# 二、最常见的 intrinsic function：`!Ref`

## 1️⃣ flow（一行写法，最常见）

```yaml
InstanceType: !Ref InstanceType
```

### 解释

* `!Ref` = 引用
* `InstanceType` = Parameters 里定义的参数名
  👉 含义：**这里的值 = 部署时你传进来的 InstanceType**

---

## 2️⃣ block（多行写法，等价）

```yaml
InstanceType:
  !Ref InstanceType
```

### 解释

* 这是 **block 方式写 intrinsic function**
* 语义和上面一模一样

✅ **CloudFormation 解析结果完全相同**
❌ 只是写法不同

---

# 三、`!Select` + `!Ref`（你模板里真正用到的）

你模板里的这一行：

```yaml
CidrBlock: !Select [0, !Ref DbSubnetIpBlocks]
```

我们一步一步拆。

---

## 1️⃣ `DbSubnetIpBlocks` 是什么？

```yaml
DbSubnetIpBlocks:
  Type: CommaDelimitedList
  Default: "10.0.48.0/24, 10.0.112.0/24, 10.0.176.0/24"
```

👉 CloudFormation 会把它变成一个 **列表**：

```text
[
  "10.0.48.0/24",
  "10.0.112.0/24",
  "10.0.176.0/24"
]
```

---

## 2️⃣ `!Select` 在干嘛？

```yaml
!Select [0, list]
```

含义是：

> **从 list 里，取 index = 0 的那个元素**

---

## 3️⃣ 合在一起读（人话版）

```yaml
CidrBlock: !Select [0, !Ref DbSubnetIpBlocks]
```

等价于：

```text
CidrBlock = DbSubnetIpBlocks[0]
```

也就是：

```text
CidrBlock = "10.0.48.0/24"
```

---

# 四、把它改写成“纯 block 写法”（重点）

刚才那行是 **flow 写法**，完全等价的 **block 写法**是：

```yaml
CidrBlock:
  !Select
    - 0
    - !Ref DbSubnetIpBlocks
```

### 对照理解

| flow | block    |
| ---- | -------- |
| `[]` | `-` list |
| 一行   | 多行缩进     |
| 不直观  | 更清晰      |

👉 **CloudFormation 内部解析结果一样**

---

# 五、你模板里 3 个 Subnet 的真实含义

```yaml
DbSubnet1:
  CidrBlock: !Select [0, !Ref DbSubnetIpBlocks]

DbSubnet2:
  CidrBlock: !Select [1, !Ref DbSubnetIpBlocks]

DbSubnet3:
  CidrBlock: !Select [2, !Ref DbSubnetIpBlocks]
```

等价于伪代码：

```python
DbSubnetIpBlocks = [
  "10.0.48.0/24",
  "10.0.112.0/24",
  "10.0.176.0/24"
]

DbSubnet1.CidrBlock = DbSubnetIpBlocks[0]
DbSubnet2.CidrBlock = DbSubnetIpBlocks[1]
DbSubnet3.CidrBlock = DbSubnetIpBlocks[2]
```

---

# 六、为什么要讲 “block + intrinsic function”

因为 **这 3 件事最容易搞混**：

1. intrinsic function ≠ YAML 语法（是 CloudFormation 的）
2. intrinsic function **既能 flow 写，也能 block 写**
3. block 写法一旦缩进错 → 直接 template invalid / CREATE_FAILED

---

# 七、面试 / 考试一句话版（记这个）

> **CloudFormation 的 intrinsic functions（如 !Ref、!Select）既可以用 flow 写法一行表示，也可以用 block 写法通过缩进和 list 表达，语义完全相同，只是 YAML 表达形式不同。**

---

如果你愿意，下一步我可以：

* 🔍 用你这份模板 **画一张“参数 → intrinsic function → 资源”的数据流图**
* 🧪 出 3 道 **!Ref / !Select / !GetAtt 的考试陷阱题**
* 🧠 帮你把所有常见 intrinsic function 做成一张 **对照速记表**
