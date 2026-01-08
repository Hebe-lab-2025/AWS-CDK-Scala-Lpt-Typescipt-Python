下面是 **SAA-C03 考试专用 · Messaging（SQS / SNS）秒杀材料**
目标：👉 **看到题干 3 秒判断：SQS 还是 SNS**

---

# 🔥 Part 1：10 道 **SQS / SNS** 陷阱真题（真题风格）

> 规则：
> **先抓“消息模型” → 再选服务**

---

### Q1

题干：需要 **异步处理，保证任务一定被处理**
✅ **SQS**
👉 Queue = **At-least-once delivery**

---

### Q2

题干：一个事件要 **同时通知多个系统**
✅ **SNS**
👉 Fan-out 广播模型

---

### Q3

题干：消费者处理慢，消息不能丢
✅ **SQS**
👉 Message retention + visibility timeout

---

### Q4

题干：需要 **推送到 Email / SMS / Lambda**
✅ **SNS**
👉 SNS = 多协议推送

---

### Q5

题干：Web 层不能被耗时任务拖慢
✅ **SQS**
👉 解耦 Web → Worker

---

### Q6

题干：一个订单事件，要触发 **库存 / 物流 / 通知**
✅ **SNS**
👉 一个发布者 → 多订阅者

---

### Q7

题干：需要 **削峰填谷（buffer traffic spikes）**
✅ **SQS**
👉 Queue = 流量缓冲器

---

### Q8

题干：消息必须 **按顺序处理**
✅ **SQS FIFO**
👉 SNS 不保证顺序

---

### Q9

题干：生产者不关心谁消费
✅ **SNS**
👉 Pub/Sub 解耦

---

### Q10

题干：Worker 数量需要根据积压消息自动扩容
✅ **SQS**
👉 根据 queue depth 触发 Auto Scaling

---

## 🧠 一句话终极口诀

> **要“排队慢慢干” → SQS**
> **要“一声令下全都知道” → SNS**

---

# 🧭 Part 2：**解耦架构完整决策脑图（ASCII）**

```text
                    Event / Message
                           |
        ------------------------------------------------
        |                                              |
     一对多？                                       一对一？
        |                                              |
      YES                                            YES
        |                                              |
      SNS                                            SQS
        |                                              |
  Fan-out / Broadcast                         Queue / Buffer
        |
  -------------------------
  |           |           |
 Email       Lambda     SQS订阅
```

---

### 📌 **SNS + SQS 组合（考试高频）**

```text
        Publisher
            |
           SNS
        /    |     \
     SQS   SQS    SQS
   Worker Worker Worker
```

👉 **用途**：

* 同一事件
* 多系统
* 各自异步处理
* 互不影响

---

# 📄 Part 3：**SAA-C03 Messaging 一页 Cheat Sheet**

## 🔹 SQS（Queue）

* Pull 模型（消费者拉）
* 保证投递（At-least-once）
* 支持 **FIFO**
* 削峰填谷
* 解耦 Web / Worker
* 适合 **后台任务 / 批处理**

---

## 🔹 SNS（Topic）

* Push 模型
* Fan-out 广播
* 多订阅者
* 支持 Email / SMS / Lambda / SQS
* 不保证顺序
* 适合 **事件通知**

---

## ❌ 高频混淆点（必记）

| 误区            | 正解 |
| ------------- | -- |
| SNS 能当队列      | ❌  |
| SQS 能广播       | ❌  |
| SNS 保证顺序      | ❌  |
| SQS = Pub/Sub | ❌  |

---

## ⚡ 3 秒考试决策树

```text
【Step 1】是不是一个消息给多个系统？
   └─ YES → SNS

【Step 2】是不是需要缓冲 / 排队 / 不丢任务？
   └─ YES → SQS

【Step 3】是不是两者都有？
   └─ SNS + SQS
```

---

## 🎯 考试生存法则（非常重要）

* **SNS = 通知，不负责干活**
* **SQS = 干活，不负责通知**
* 看见 **decouple / async / buffer** → 先想 SQS
* 看见 **fan-out / notify / publish** → 直接 SNS

---

如果你愿意，下一步我可以直接继续👇

* 🔥 **10 道 SNS + SQS + Lambda 组合陷阱题**
* 🧪 **SQS FIFO vs Standard 魔鬼对比题**
* 🧠 **Messaging × Auto Scaling × Retry 专项决策树**
* 📘 **Messaging 章节 PDF（考前 15 分钟速刷）**

你点一个，我马上继续。
