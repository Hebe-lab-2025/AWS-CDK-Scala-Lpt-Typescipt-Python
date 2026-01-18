下面我**按作用 + 行为逻辑**给你把这段代码解释清楚（偏实验 / 面试理解，不是生产写法）。

---

## 一、这段代码在做什么（一句话）

👉 **用 Python（boto3）向一个 SNS Topic 发消息，并通过 MessageAttributes 让不同的 SQS 订阅者“按条件接收消息”**。

---

## 二、逐块解释代码逻辑

### 1️⃣ 创建 SNS 客户端

```python
client = boto3.client(
    'sns',
    region_name='us-east-1',
    aws_access_key_id="...",
    aws_secret_access_key="..."
)
```

**作用：**

* 创建一个 **SNS API 客户端**
* 指定 region：`us-east-1`
* 用 Access Key 进行认证

⚠️ **注意（非常重要）**

* **真实项目/面试中绝对不能硬编码 AK/SK**
* 正确做法：

  * IAM Role（Lambda / EC2）
  * 或环境变量

---

### 2️⃣ SNS Topic ARN

```python
Topic_ARN = "arn:aws:sns:us-east-1:221110133360:CL_SNS_Topic"
```

**作用：**

* 指定消息要发布到的 **SNS Topic**
* 所有订阅了这个 Topic 的 SQS / Lambda 都有机会收到消息

---

### 3️⃣ send_message 函数（核心逻辑）

```python
def send_message(dest, text):
```

#### 📌 (1) 构造消息体（Message）

```python
payload = {"Message": text or "", "Destination": dest or ""}
message_str = json.dumps(payload)
```

* **Message**：真正发送到 SNS 的消息内容（字符串）
* 这里用 JSON 包装，方便下游解析

---

#### 📌 (2) 构造 MessageAttributes（关键点）

```python
attrs = {}
if dest:
    attrs['Destination'] = {'DataType': 'String', 'StringValue': dest}
if text:
    attrs['MessageBody'] = {'DataType': 'String', 'StringValue': text}
```

**这是 SNS → SQS 过滤的核心机制：**

* `MessageAttributes` 用于 **Subscription Filter Policy**
* SQS 订阅时可以配置：

  ```json
  {
    "Destination": ["CL_Queue_1"]
  }
  ```
* 只有匹配的队列才会收到消息

👉 **这就是 SNS Fanout + 条件路由**

---

#### 📌 (3) 发布消息

```python
response = client.publish(
    TopicArn=Topic_ARN,
    Message=message_str,
    MessageAttributes=attrs or None
)
```

* `publish`：向 SNS Topic 发送消息
* SNS 决定 **哪些订阅者能收到**

---

#### 📌 (4) 返回请求结果

```python
return [
    response.get('ResponseMetadata').get('RequestId'), 
    response.get('ResponseMetadata').get('HTTPStatusCode')
]
```

* 用于确认：

  * 请求是否成功（200）
  * RequestId（调试 / 日志）

---

## 三、三次发送消息分别发生了什么？

### ✅ 1️⃣ 只给 `CL_Queue_1`

```python
send_message('CL_Queue_1', 'Hello! This is a message for queue 1')
```

* MessageAttributes：

  ```
  Destination = CL_Queue_1
  ```
* **只有 filter 匹配 CL_Queue_1 的 SQS 会收到**

---

### ✅ 2️⃣ 只给 `CL_Queue_2`

```python
send_message('CL_Queue_2', 'Hello! This is a message for queue 2')
```

* 同理，只投递给 Queue 2

---

### ✅ 3️⃣ 广播给所有（逻辑上的）

```python
send_message('All', 'Hello! This message will be sent to all queues')
```

* 前提是：

  * 所有订阅者的 filter policy **包含 `All`**
* 否则不会真的“全发”

👉 **SNS 没有真正的“广播关键字”，一切靠 Filter Policy**

---

## 四、面试一句话总结（非常好用）

> This code publishes messages to an SNS topic and uses **MessageAttributes + subscription filter policies** to route messages to specific SQS queues, enabling **fanout with conditional delivery**.

---

## 五、如果你愿意，我可以继续帮你

* 🔐 把这段代码改成 **生产级安全版本（IAM Role + env）**
* 🧠 画一张 **SNS → 多 SQS + Filter Policy 白板图**
* 🧪 出 5 道 **SNS Filter Policy 高频面试陷阱题**
* 🔄 对比：**SNS Filter vs SQS 消费端过滤（为什么前者更省钱）**

你选一个，我直接给你。
