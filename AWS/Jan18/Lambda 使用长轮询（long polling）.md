By default, Lambda uses long polling and waits till the SQS queue sends a message.

**翻译：**

默认情况下，**Lambda 使用长轮询（long polling）**，并且会一直等待，直到 **SQS 队列中有消息可供处理**。

---

**解释（好记版）：**

* **long polling（长轮询）**：
  Lambda 不会频繁“空转”去查队列，而是**持续等待一段时间**，直到有消息或超时。
* **效果**：

  * ✅ 减少无效请求
  * ✅ 降低延迟
  * ✅ 更省成本
* **本质**：
  👉 **SQS 有消息 → Lambda 立刻被触发**

---

**一句话面试总结：**

> Lambda 默认通过 **SQS 长轮询** 被动等待消息，实现 **低延迟、低成本的事件驱动处理**。



