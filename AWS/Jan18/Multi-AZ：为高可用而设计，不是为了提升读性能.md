**翻译：**

**Multi-AZ 部署** 允许我们将数据存储在 **多个可用区（Availability Zones）** 中，以实现 **容错性（fault tolerance）**。
在另一个可用区中创建的 **辅助数据库（secondary database）** 包含主数据库中的 **全部数据**，并且处于 **待命（dormant）状态**。

---

**补充解释（好理解版）：**

* **Multi-AZ**：为高可用而设计，不是为了提升读性能
* **Secondary / Standby**：

  * 与主库实时同步
  * 不对外提供读写
  * 主库故障时自动接管（failover）

**一句话总结：**

> Multi-AZ 通过跨可用区的主备同步，实现高可用和自动故障切换。


**Standby 的含义（AWS 语境）**

### 中文

* **备用**
* **待命**
* （RDS 场景常说）**备用实例 / 待命实例**

---

### 在 RDS Multi-AZ 中的准确含义

**Standby** 指的是：

> 一个 **已经创建并持续同步数据，但不对外提供服务的备用数据库实例**。

**关键点：**

* ✅ 数据与主库实时同步
* ❌ 不能直接访问（不读、不写）
* ❌ 不参与负载
* ✅ 主库故障时 **自动接管（failover）**

---

### 一句话面试版

> **Standby is a synchronized, inactive backup instance that automatically takes over during failures.**

---

### 易混提醒

* ❌ Standby ≠ Backup
* ❌ Standby ≠ Read Replica

如果你想，我可以用一句话把 **Standby / Backup / Read Replica** 三个彻底区分开。

