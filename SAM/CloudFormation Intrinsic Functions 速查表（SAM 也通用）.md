## 🧠 CloudFormation Intrinsic Functions 速查表（SAM 也通用）

| 写法                                    | 作用           | 最常见用途                        | 易错点                                     |
| ------------------------------------- | ------------ | ---------------------------- | --------------------------------------- |
| `!Ref X`                              | 引用资源/参数      | 取参数值、取资源“默认返回值”              | **不同资源返回值不同**（有的是 Name，有的是 ID，有的是物理资源名） |
| `!GetAtt X.Arn`                       | 取资源属性        | Lambda ARN、Role ARN、LB DNS 等 | 属性名必须对（`Arn`/`DomainName`/`DnsName` 等）  |
| `!Sub "..."`                          | 字符串模板替换      | 拼 ARN / URL                  | `${Var}` 里可用参数/伪参数/资源；复杂时用映射形式          |
| `!Join [":", [...]]`                  | 拼字符串         | 拼 ARN、拼路径                    | 大多时候 `!Sub` 更直观                         |
| `!Select [i, list]`                   | 取第 i 个       | 取 AZ 列表、分片列表                 | index 从 0 开始，越界直接炸                      |
| `!Split [",", str]`                   | 拆字符串         | 把 CSV 参数变成 list              | 注意空格、分隔符                                |
| `!If [Cond, A, B]`                    | 条件分支         | prod/dev 资源差异                | 依赖 `Conditions:` 先定义                    |
| `!Equals [A, B]`                      | 条件表达式        | env 是否为 prod                 | 只能用于 Conditions/If 里                    |
| `!And / !Or / !Not`                   | 逻辑组合         | 复杂条件                         | 只在 Conditions 里用                        |
| `!FindInMap [Map, TopKey, SecondKey]` | 映射查值         | region→AMI、env→配置            | Map 的 key 必须存在                          |
| `!ImportValue X`                      | 跨 Stack 引用输出 | 复用 VPC/Subnets               | Export name 必须唯一且存在                     |
| `!Base64`                             | Base64 编码    | EC2 UserData                 | 一般只给 EC2 用                              |
| `!Cidr [ip, count, mask]`             | 生成 CIDR 列表   | VPC 子网规划                     | 输出是 list，常配 `!Select`                   |
| `!GetAZs ""`                          | 取 AZ 列表      | 多 AZ 子网                      | 传 `""` 表示当前 Region                      |

---

## 🧪 5 道 `!Ref / !GetAtt` 真·易错题（附标准答案）

### 1) `!Ref AWS::Serverless::Function` 返回什么？是 ARN 吗？

**答案：不是 ARN。**
`!Ref` 对 Lambda Function 通常返回 **函数名（FunctionName）** 或 **物理资源名**（如果没显式写 FunctionName，会生成一个名字）。ARN 用 `!GetAtt Func.Arn`。

---

### 2) `!GetAtt MyApi.Arn` 能用在 `AWS::Serverless::Api` 吗？

**答案：大概率不行/没这个属性。**
API Gateway 的 ARN/URL 不是这样取的；**拼 URL 常用 `!Sub`**：
`!Sub "https://${MyApi}.execute-api.${AWS::Region}.amazonaws.com/${Stage}/..."`
（这里 `${MyApi}` 依赖 `!Ref` 返回 API 的 **RestApiId**）

---

### 3) `!Ref AWS::Serverless::Api` 返回的是 URL 吗？

**答案：不是 URL。**
返回的是 **RestApiId**（形如 `a1b2c3d4e5`）。URL 需要你用 `!Sub` 拼。

---

### 4) 想在 IAM Policy 里授权一张 DynamoDB 表，你写了：

```yaml
Resource: !Ref CoursesTable
```

这一定对吗？
**答案：不一定。**
对 `AWS::DynamoDB::Table`，`!Ref` 返回 **表名**，但 IAM `Resource` 需要 **表 ARN**。正确写法：

```yaml
Resource: !GetAtt CoursesTable.Arn
```

（或 `!Sub arn:aws:dynamodb:${AWS::Region}:${AWS::AccountId}:table/${TableName}`）

---

### 5) 你把 `!GetAtt SomeRole.Arn` 写在 `Policies:` 里当作 managed policy ARN，会发生什么？

**答案：概念错位。**
`Policies`（SAM 的简写）可以接 **Policy Template** / **ManagedPolicy** / **Inline Statement**，但 **Role ARN ≠ Policy ARN**。要么写 inline statement，要么在 Role 上挂 managed policy。

---

## 🔍 把“模板里每个 `!Ref` 的真实返回值”标出来（基于我上次给你的示例模板）

你之前那份示例里有这些 `!Ref`：

### 1) `RestApiId: !Ref CoursesApi`

* `CoursesApi` 类型：`AWS::Serverless::Api`
* ✅ `!Ref CoursesApi` 真实返回：**API Gateway RestApiId**（例如 `a1b2c3d4e5`）
* 用途：把 Lambda 的 `Events` 绑定到“这个 API”

### 2) `!Sub` 里用到 `${CoursesApi}`

```yaml
!Sub "https://${CoursesApi}.execute-api.${AWS::Region}.amazonaws.com/dev/courses"
```

* 这里的 `${CoursesApi}` 本质上等价于 `!Ref CoursesApi`
* ✅ 实际值仍然是 **RestApiId**
* 拼出来就是：`https://<RestApiId>.execute-api.<region>.amazonaws.com/dev/courses`

### 3) （如果你模板里出现）`!Ref AWS::Region / AWS::AccountId`

* 这是伪参数
* ✅ `!Ref AWS::Region` 返回当前 region（例如 `us-west-2`）
* ✅ `!Ref AWS::AccountId` 返回账号 ID（例如 `123456789012`）

---

## 如果你要我“对你真实 template.yaml”逐个标注

把你的 `template.yaml` 粘出来（或至少 `Resources:` 那段）。我会按**每一个 `!Ref`** 输出这种清单：

* `!Ref X` → 实际返回值类型（Name/Id/Arn/ParameterValue）
* 返回值示例长什么样
* 该值在哪些地方**能用/不能用**（比如 IAM Resource 必须 ARN）
