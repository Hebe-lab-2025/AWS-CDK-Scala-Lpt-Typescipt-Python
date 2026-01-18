## Build a Fanout Serverless Architecture with **SNS + SQS + Lambda**

![Image](https://miro.medium.com/1%2A9z3sbaE6yGT1Ukau8iq4ew.png)

![Image](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2017/07/25/messaging-fanout-for-serverless-with-sns-diagram2.png)

![Image](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2017/07/25/messaging-fanout-for-serverless-with-sns-diagram1.png)

![Image](https://miro.medium.com/1%2AKh1tAdP30ErlOrOqY-zlLw.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A9z3sbaE6yGT1Ukau8iq4ew.png)

![Image](https://www.jeremydaly.com/images/uploads/2019/01/sns-sqs-app.png)

### 1️⃣ What problem does Fanout solve?

Fanout lets **one event** trigger **multiple independent consumers** in parallel.
You publish **once** to SNS → SNS **replicates** the message → each subscriber processes it **asynchronously** and **independently**.

---

### 2️⃣ Core components & roles

| Service             | Responsibility                         |
| ------------------- | -------------------------------------- |
| **SNS Topic**       | Entry point; broadcasts events         |
| **SQS Queues (×2)** | Durable buffers for each consumer      |
| **Lambda (×2)**     | Business logic per consumer            |
| **CloudWatch**      | Metrics, logs, retries, DLQ monitoring |

---

### 3️⃣ High-level flow (exam-friendly)

```
Producer
   ↓
SNS Topic
   ↓ (fanout)
SQS Queue A → Lambda A
SQS Queue B → Lambda B
```

* SNS **pushes** to SQS
* Lambda **polls** SQS
* Each path scales and fails **independently**

---

### 4️⃣ Step-by-step build (what the lab does)

#### Step 1: Create SNS Topic

* Standard topic
* Used as the **fanout hub**

#### Step 2: Create 2 SQS Queues

* Standard queues (default)
* Optional: enable **DLQ** (exam bonus)

#### Step 3: Subscribe SQS to SNS

* Subscription type: **SQS**
* Update **SQS access policy** to allow SNS to publish

#### Step 4: Create 2 Lambda Functions

* Each Lambda is triggered by **one SQS queue**
* Event source mapping handles:

  * batch size
  * retries
  * visibility timeout

#### Step 5: Publish a message to SNS

* SNS delivers **a copy** to each SQS
* Both Lambdas run **in parallel**

#### Step 6: Observe in CloudWatch

* Lambda invocations
* SQS `NumberOfMessagesReceived`
* Error / retry behavior

---

### 5️⃣ Message filtering (very testable)

SNS can **filter messages per subscriber**:

```json
{
  "eventType": ["order_created"]
}
```

* Filter happens **at SNS**
* Saves cost (messages never hit SQS)

---

### 6️⃣ Why SNS + SQS (not SNS → Lambda directly)?

| SNS → Lambda   | SNS → SQS → Lambda         |
| -------------- | -------------------------- |
| No buffering   | **Durable buffer**         |
| Risk of loss   | **At-least-once delivery** |
| Tight coupling | **Loose coupling**         |
| Hard retries   | **Built-in retries + DLQ** |

👉 **Exam rule**: If reliability matters → add SQS.

---

### 7️⃣ Common SAA-C03 traps

❌ Forgetting SQS policy → SNS publish fails
❌ Visibility timeout < Lambda timeout
❌ No DLQ for poison messages
❌ Assuming SNS guarantees order (it doesn’t)

---

### 8️⃣ When to use this pattern

✅ Event-driven systems
✅ Multiple downstream processors
✅ Different retry / scaling needs
✅ Loose coupling & fault isolation

---

### 9️⃣ One-sentence exam answer

> *“Use SNS for fanout to multiple SQS queues so each Lambda consumer processes events asynchronously with durability, retries, and independent scaling.”*

If you want, I can next:

* 🧠 Draw an **exam whiteboard version**
* 🧪 Give **5 real SAA-C03 fanout questions**
* 🔍 Compare **SNS fanout vs EventBridge**
