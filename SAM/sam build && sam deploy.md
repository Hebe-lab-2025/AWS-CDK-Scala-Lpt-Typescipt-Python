你这次 `sam build && sam deploy ...` **已经成功部署完成了**，而且从输出看：**只创建了一个 DynamoDB 表**，没有创建 Lambda / API Gateway（这点很关键）。

---

## 你这段输出在说什么（逐段拆）

### 1) Build 阶段

* `Build Succeeded`
* 产物在：

  * `Built Artifacts: .aws-sam/build`
  * `Built Template: .aws-sam/build/template.yaml`

✅ 说明：本地打包/编译没问题。

---

### 2) Deploy 参数（SAM 用什么配置部署）

你指定了：

* Stack：`clab-stack`
* Region：`us-east-1`
* S3 bucket：`sam-clab-bucket-097285037130`
* Capabilities：`CAPABILITY_IAM`

✅ 说明：模板/代码会先上传到你指定 bucket，然后 CloudFormation 用它来创建资源。

---

### 3) Changeset 里将要新增的资源

Changeset 显示只有一条：

* `+ Add CourseTable (AWS::DynamoDB::Table)`

✅ 结论：**这次部署只新增 DynamoDB 表 `CourseTable`**。

---

### 4) CloudFormation 事件（创建过程）

* `CourseTable`：`CREATE_IN_PROGRESS → CREATE_COMPLETE`
* `clab-stack`：`CREATE_COMPLETE`

✅ 表创建成功、Stack 也成功结束。

---

### 5) Stack Outputs（输出）

输出键值：

* `DynamoDBTableName = Courses`

✅ 说明：你模板里把表名设置成了 `Courses`，并把它作为 output 打印出来。

---

## 最重要的点：为什么没有 API Gateway / Lambda？

因为 changeset 和 events 里 **完全没有**：

* `AWS::Lambda::Function`
* `AWS::ApiGateway::RestApi` / `AWS::ApiGatewayV2::*`

👉 所以不是“没创建成功”，而是 **你的 template.yaml 里根本没有定义它们**（或者条件/参数导致没有被包含）。

---

## 你接下来可以做的 3 件事（直接可用命令）

### A) 验证 stack 里到底有哪些资源（最直接）

```bash
aws cloudformation list-stack-resources --stack-name clab-stack --region us-east-1
```

你会看到目前基本只有 DynamoDB 表（再加一些隐含资源的话也会列出来）。

---

### B) 看一下 SAM 实际部署用的模板（定位为啥没 Lambda）

```bash
sed -n '1,200p' .aws-sam/build/template.yaml
```

重点搜这几个关键词：

```bash
grep -n "AWS::Lambda::Function\|AWS::Serverless::Function\|AWS::Serverless::Api\|AWS::ApiGateway" -n .aws-sam/build/template.yaml
```

如果 grep 没输出 → 模板里确实没有 API/Lambda。

---

### C) 你想删掉这次部署（清理）

```bash
sam delete --stack-name clab-stack --region us-east-1
```

---

## 一句话总结

你这次部署 **100% 成功**；CloudFormation 只创建了 **DynamoDB 表 `Courses`**，因为模板里（从 changeset 判断）**没有定义 Lambda/API**。

如果你把 `template.yaml`（或 `.aws-sam/build/template.yaml`）贴出来，我可以直接帮你指出：**为什么只生成了 DynamoDB**，以及怎么改回 Hello World API。
