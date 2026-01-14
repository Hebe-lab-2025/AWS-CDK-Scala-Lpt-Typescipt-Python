很好，这两个词**在 CloudFormation / SAM 里是必考基础**。我给你 **极简 + 准确 + 能马上用** 的解释。

---

## 一、什么是「语法糖」（syntactic sugar）？

### 一句话定义

> **语法糖 = 写起来更简单，但本质没变的写法。**

它**不增加新能力**，只是：

* 少写
* 更好读
* 不容易写错

---

### 在 SAM 里的例子（非常典型）

#### 🍬 SAM 写法（语法糖）

```yaml
Type: AWS::Serverless::SimpleTable
PrimaryKey:
  Name: ID
  Type: Number
```

#### 🍭 CloudFormation 本质写法（展开后）

```yaml
Type: AWS::DynamoDB::Table
AttributeDefinitions:
  - AttributeName: ID
    AttributeType: N
KeySchema:
  - AttributeName: ID
    KeyType: HASH
BillingMode: PAY_PER_REQUEST
```

👉 **SAM 帮你把复杂的 CFN 写法“自动翻译”了**。
这就是语法糖。

---

### 再来一个你正在用的例子

```yaml
Globals:
  Function:
    Runtime: nodejs18.x
```

👉 本质等价于：**每个 Lambda 都单独写 Runtime**
只是 SAM 帮你集中写了一次。

---

## 二、什么是 `!Ref`？

### 一句话定义

> **`!Ref` = 引用另一个资源的“默认返回值”**

它是 **CloudFormation 的 intrinsic function（内置函数）**。

---

## 三、`!Ref` 在你模板里的真实含义

### 你现在的用法

```yaml
COURSE_TABLE: !Ref CourseTable
```

### 实际发生了什么？

* `CourseTable` 是一个 **资源逻辑名**
* `!Ref CourseTable` 会在部署时被替换成：

  * **DynamoDB 表名**（这里是 `Courses`）

👉 所以 Lambda 运行时看到的是：

```bash
COURSE_TABLE=Courses
```

---

## 四、`!Ref` 不同资源，返回值不同（重点）

这是面试爱考点。

| 资源类型                           | `!Ref` 返回什么 |
| ------------------------------ | ----------- |
| `AWS::Serverless::SimpleTable` | 表名          |
| `AWS::DynamoDB::Table`         | 表名          |
| `AWS::Serverless::Function`    | 函数名         |
| `AWS::Lambda::Function`        | 函数名         |
| `AWS::S3::Bucket`              | bucket 名    |
| `AWS::IAM::Role`               | role 名      |
| Parameter                      | 参数值         |

👉 **不是一律返回 ARN**，这是大坑。

---

## 五、`!Ref` vs `!GetAtt`（顺手对比）

### `!Ref`

```yaml
!Ref CourseTable
```

* 拿“默认值”
* 简单、常用

### `!GetAtt`

```yaml
!GetAtt GetCourses.Arn
```

* 拿**指定属性**
* 常用于 ARN、URL、Endpoint

---

## 六、为什么 `!Ref` 要用 `!`？

这是 YAML 的 **tag 语法**：

* `!Ref` 是 `Ref:` 的简写
* 等价写法：

```yaml
Ref: CourseTable
```

---

## 七、超短背诵版（考试 / 面试）

> **语法糖**：写得更简单，本质不变。
> **`!Ref`**：引用资源的默认返回值（常是名字，不一定是 ARN）。

---

如果你愿意，我可以下一步帮你：

* 🧠 列一张 **CloudFormation intrinsic function 速查表**
* 🧪 出 5 道 **`!Ref` / `!GetAtt` 真·易错题**
* 🔍 把你模板里每个 `!Ref` 的真实返回值标出来
