下面是 **SAA-C03 Messaging 决胜包**，专打 **SNS / SQS / Lambda** 的混合陷阱，目标：**看到关键词直接反射答案**。

---

# 🔥 一、10 道 SNS + SQS + Lambda 组合陷阱题（秒杀逻辑）

### Q1

**场景**：多个系统需要同时收到事件
**秒选**：✅ **SNS（Fan-out）**
👉 **暗号**：one → many

---

### Q2

**场景**：需要解耦 + 异步处理
**秒选**：✅ **SQS**
👉 **暗号**：decouple / buffer

---

### Q3

**场景**：消息处理慢，流量突增
**正确架构**：
**SNS → SQS → Lambda** ✅
👉 **暗号**：burst traffic = queue

---

### Q4

**场景**：Lambda 直接订阅 SNS，但失败会丢消息
**修正**：

* ❌ 重试代码
* ✅ **SNS → SQS → Lambda**（有持久化）

---

### Q5

**场景**：同一消息必须只处理一次
**秒选**：

* ❌ Standard
* ✅ **FIFO + 去重**

---

### Q6

**场景**：顺序不重要，但吞吐量极高
**秒选**：

* ✅ **SQS Standard**

---

### Q7

**场景**：Lambda 幂等性问题
**正确理解**：

* ❌ Lambda 只会执行一次
* ✅ **必须自己保证幂等**

---

### Q8

**场景**：下游系统偶尔失败
**最佳方案**：

* ✅ **SQS Redrive Policy + DLQ**

---

### Q9

**场景**：需要指数退避重试
**秒选**：

* ✅ **SQS Visibility Timeout + Retry**

---

### Q10

**一句话总结**

> **SNS 负责广播，SQS 负责缓冲，Lambda 负责计算**

---

# 🧪 二、SQS FIFO vs Standard（魔鬼对比题）

| 维度   | Standard | FIFO    |
| ---- | -------- | ------- |
| 顺序   | ❌ 不保证    | ✅ 严格    |
| 去重   | ❌ 可能重复   | ✅ 内建    |
| 吞吐   | ✅ 极高     | ❌ 较低    |
| 延迟   | 低        | 略高      |
| 使用场景 | 日志 / 任务  | 订单 / 金融 |

### 秒杀口诀

* **“顺序 + 只一次” → FIFO**
* **“量大 + 快” → Standard**

---

# 🧠 三、Messaging × Auto Scaling × Retry 决策树（考试版）

```text
事件来了？
 ├─ 要广播？ ──> SNS
 │              └─ 要缓冲？ ──> 接 SQS
 │                              └─ 计算？ ──> Lambda
 │
 └─ 不广播？
     └─ 只解耦？ ──> SQS → Lambda
```

### Retry & Scaling 决策

```text
失败了？
 ├─ 偶发失败 ──> 自动 Retry
 ├─ 持续失败 ──> DLQ
 └─ 流量突增 ──> Lambda 并发 ↑（SQS 驱动）
```

👉 **暗号**：

* queue depth ↑ = Lambda scale
* retry ≠ 重复消费（要幂等）

---

# 📘 四、Messaging 章节 PDF（考前 15 分钟速刷）

### PDF 结构（极致压缩）

1. **一页对比表**：SNS / SQS / Lambda
2. **FIFO vs Standard 秒选表**
3. **5 个必错陷阱**
4. **三行架构口诀**
5. **15 秒口述版背诵稿**

### 15 秒口述稿（临考）

> “广播用 SNS，解耦用 SQS；
> 流量大先排队；
> 顺序和去重才用 FIFO；
> 失败进 DLQ；
> Lambda 一定要幂等。”

---

## 🚀 下一步我可以直接帮你：

* 🔥 **再出 20 道 Messaging 盲选题（只看暗号）**
* 🧪 **做一整套 SAA-C03 Messaging 模拟卷**
* 📄 **把这套生成 A4 / 手机版 PDF**

你选一个，我继续。
