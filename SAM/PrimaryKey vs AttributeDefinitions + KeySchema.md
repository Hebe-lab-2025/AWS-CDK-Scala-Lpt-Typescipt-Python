下面把你这两点差异**讲透**，并补上“怎么从 SAM 写法翻译成 CloudFormation 写法”的直观映射。

---

## 1) PrimaryKey vs AttributeDefinitions + KeySchema

### ✅ SAM（更抽象、更省事）

在 SAM 里你可以用更高级的字段，比如（示意）：

```yaml
PrimaryKey:
  Name: ID
  Type: Number
```

SAM 会帮你**自动生成** CloudFormation 里必须写的两块：

* `AttributeDefinitions`：声明 key 属性的名字 + DynamoDB 类型（S/N/B）
* `KeySchema`：声明这个属性是 `HASH`（partition key）还是 `RANGE`（sort key）

---

### ✅ CloudFormation（更底层、更显式）

你必须写成这样：

```yaml
AttributeDefinitions:
  - AttributeName: ID
    AttributeType: N

KeySchema:
  - AttributeName: ID
    KeyType: HASH
```

如果你有 sort key，比如 `SK`：

```yaml
AttributeDefinitions:
  - AttributeName: PK
    AttributeType: S
  - AttributeName: SK
    AttributeType: S

KeySchema:
  - AttributeName: PK
    KeyType: HASH
  - AttributeName: SK
    KeyType: RANGE
```

### 一句话原因

> SAM 用 `PrimaryKey` 是为了让你不用记 DynamoDB 那套底层 schema 语法；CloudFormation 要求你显式写全底层字段。

---

## 2) BillingMode 默认 PAY_PER_REQUEST vs ProvisionedThroughput 必填规则

### ✅ SAM 的默认行为（serverless 友好）

SAM 模板里，如果你**不提供** `ProvisionedThroughput`，它倾向于：

* 自动选择 `BillingMode: PAY_PER_REQUEST`

这适合 serverless，因为：

* 不用预估读写 capacity
* 不用管扩缩容
* 费用按实际请求计

---

### ✅ CloudFormation 的硬规则（必须匹配）

CloudFormation 的规则是：

* 如果 `BillingMode: PAY_PER_REQUEST`

  * **不能写** `ProvisionedThroughput`（写了可能报错/不被允许）
* 如果 `BillingMode: PROVISIONED`

  * **必须写** `ProvisionedThroughput`

    * `ReadCapacityUnits`
    * `WriteCapacityUnits`

示例（PROVISIONED）：

```yaml
BillingMode: PROVISIONED
ProvisionedThroughput:
  ReadCapacityUnits: 5
  WriteCapacityUnits: 5
```

示例（PAY_PER_REQUEST）：

```yaml
BillingMode: PAY_PER_REQUEST
# 不要 ProvisionedThroughput
```

### 一句话原因

> SAM 为了 serverless 默认“按需付费”；CloudFormation 强制你把 BillingMode 和 Throughput 写得完全一致，不允许含糊。

---

## 快速对照表（背这个就够了）

| 目标    | SAM 写法               | CloudFormation 写法                                  |
| ----- | -------------------- | -------------------------------------------------- |
| 定义主键  | `PrimaryKey`         | `AttributeDefinitions` + `KeySchema`               |
| 不想管容量 | 不写 Throughput → 默认按需 | `BillingMode: PAY_PER_REQUEST` 且 **不写** Throughput |
| 要预置容量 | 写 Throughput         | `BillingMode: PROVISIONED` 且 **必须写** Throughput    |

---

如果你把你当前的 **SAM table 片段**贴出来（resource 那一段），我可以直接给你**等价的 CloudFormation DynamoDB Table 版本**（一模一样的 schema + billing）。
