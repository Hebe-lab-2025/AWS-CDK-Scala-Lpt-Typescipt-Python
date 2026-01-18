**DB instance identifier 的含义（RDS 中的标准说法）**

### 中文

👉 **数据库实例标识符**（也可理解为：数据库实例名称）

---

### 是什么？

**DB instance identifier** 是你在创建 **Amazon RDS 数据库实例** 时指定的一个：

* **唯一名称**
* 用来 **识别和管理这个 RDS 实例**
* 是 RDS 资源层级的名字（不是数据库名）

---

### 关键特性（考试高频）

* 在**同一 Region 内唯一**
* 只能包含：

  * 字母（a–z）
  * 数字（0–9）
  * 连字符（-）
* **必须以字母开头**
* 创建后 **不能修改**

---

### 常见混淆（必考）

| 容易混的概念        | 是否是 DB instance identifier |
| ------------- | -------------------------- |
| 数据库名（DB name） | ❌                          |
| Endpoint      | ❌                          |
| ARN           | ❌                          |
| RDS 实例名       | ✅                          |

---

### 示例

* `mysql8`
* `prod-orders-db`
* `test-rds-01`

---

### 一句话总结

> **DB instance identifier 是用来唯一标识一个 RDS 实例的名字，而不是数据库本身的名字。**


**Monitoring 的含义（技术语境）**

### 中文

👉 **监控** / **监控系统运行状况**

---

### 是什么？

**Monitoring** 指的是：

> **持续收集、观察和分析系统运行指标，以确保系统正常、稳定运行，并能及时发现问题。**

---

### 常监控的内容（AWS / 系统）

* **性能指标**：CPU、内存、延迟（latency）
* **负载指标**：QPS、并发数、TPS
* **可靠性指标**：错误率、失败次数
* **资源状态**：实例是否存活、磁盘空间

---

### AWS 场景一句话

> *We use CloudWatch for monitoring system performance and availability.*
> **我们使用 CloudWatch 来监控系统的性能和可用性。**

---

### 一句话总结

> **Monitoring = 用指标和日志持续观察系统是否健康，并提前发现问题。**

**uncheck 的中文意思：**

👉 **取消勾选** / **取消选中**

---

### 常见用法

* *uncheck the option* → **取消勾选该选项**
* *uncheck the box* → **取消选中复选框**

---

### UI / 操作说明里的常见翻译

> **Uncheck the box to disable the feature.**
> **取消勾选该复选框以禁用该功能。**

---

### 一句话总结

> **uncheck = 把已经打勾的选项去掉。**



