下面我把你这段“模板讲解”**按 SAM 模板真实含义**纠正+补强（因为原文里有几处容易误解的表述），并把每一段**对应到你贴的 template.yaml**。

---

## 1) Lines 5–9：Globals 是什么？为什么放这里？

你模板里是：

```yaml
Globals:
  Function:
    Timeout: 3
    Tracing: Active
    Runtime: nodejs18.x
```

含义：

* `Globals.Function` 是 **SAM 的语法糖**
* 写在这里的 `Timeout / Tracing / Runtime` 会**默认继承**到每个 `AWS::Serverless::Function`（除非函数里单独覆盖）

为什么这么做？

* 4 个 CRUD Lambda 的配置一致（timeout、runtime、tracing）
* 放在 Globals 能减少重复、降低出错率（比如某个函数忘记改 runtime）

⚠️ 小提醒：这只对 `AWS::Serverless::Function` 这种 SAM 资源生效，不是对所有 CloudFormation 资源都生效。

---

## 2) Lines 18–27：GetCourses Lambda 做了什么？

你模板的 GetCourses 资源是：

```yaml
GetCourses:
  Type: AWS::Serverless::Function
  Properties:
    CodeUri: functions/
    Handler: readCourses.readCourses
    Environment:
      Variables:
        COURSE_TABLE: !Ref CourseTable
    Role: !Sub arn:aws:iam::${AWS::AccountId}:role/ClabLambdaRole
```

它定义了一个 Lambda：

* 代码在 `functions/`
* 入口 handler 是 `readCourses.readCourses`
* 环境变量里传了表名（或表的引用）
* 执行角色用 `ClabLambdaRole`

---

## 3) Lines 21–22：CodeUri + Handler 的准确解释（这里最容易被写错）

原文说：

> CodeUri consists of the path to the handler function… handler name is readCourses

更准确的说法是：

### ✅ CodeUri

`CodeUri: functions/` 指的是：

* **Lambda 的代码包根目录**
* SAM 打包时，会把这个目录作为函数代码源

⚠️ 它不是“handler 函数的路径”，而是“代码根目录路径”。

---

### ✅ Handler（Node.js Zip 的规则）

`Handler: readCourses.readCourses` 表示：

* 第一个 `readCourses` = **模块文件名**（`readCourses.js`）
* 第二个 `readCourses` = **导出的函数名**（`exports.readCourses = ...`）

所以你的目录必须满足：

```
functions/
  readCourses.js   # exports.readCourses
```

并且 `readCourses.js` 里要有类似：

```js
exports.readCourses = async (event) => { ... }
```

否则就会报经典错误：

* `Runtime.ImportModuleError`
* `Handler not found`

---

## 4) Lines 23–25：Environment Variables 是干嘛的？

```yaml
Environment:
  Variables:
    COURSE_TABLE: !Ref CourseTable
```

含义：

* 在 Lambda 运行时注入环境变量 `COURSE_TABLE`
* 你的代码里通过 `process.env.COURSE_TABLE` 读取表名

例如：

```js
const tableName = process.env.COURSE_TABLE;
```

⚠️ 注意 `!Ref CourseTable` 的值：

* 因为 `CourseTable` 是 `AWS::Serverless::SimpleTable`，
* `!Ref` 一般会返回 **表名**（这里是 `Courses`），正好符合你想要的。

---

## 5) Lines 26–27：Role 的含义（以及原文里一个潜在误导）

```yaml
Role: !Sub arn:aws:iam::${AWS::AccountId}:role/ClabLambdaRole
```

这句的真实含义是：

* **Lambda 执行时会使用这个 IAM Role**
* Role 必须已经存在（因为你写的是 ARN，而不是让 SAM 创建）

原文说：

> attach a role… allows the table to perform basic operations

更准确：

* 是 **Lambda** 通过这个 Role 去访问 DynamoDB
* 不是 “table perform operations”，table 本身不执行操作。

---

## 6) 你接下来要看的 “role policy” 应该长什么样？

如果它要支持 CRUD，最常见是允许对表的基本操作，例如：

* `dynamodb:GetItem`
* `dynamodb:Scan` / `dynamodb:Query`
* `dynamodb:PutItem`
* `dynamodb:UpdateItem`
* `dynamodb:DeleteItem`

资源一般是：

* `arn:aws:dynamodb:REGION:ACCOUNT_ID:table/Courses`

---

## 一个关键提醒（你后面一定会遇到）

你目前这份模板里 **没有 Events / API Gateway**，所以就算 Lambda 创建成功：

* 控制台能看到函数
* 但你没有 URL 直接调用
* 只能用 `aws lambda invoke` 或后续再加 API 触发器

---

如果你把“role policy snippet”那段也贴出来，我可以帮你做两件事：

1. 检查权限是否刚好够（最小权限 / least privilege）
2. 指出是否缺了 DynamoDB CRUD 某些 action（避免运行时报 AccessDenied）
