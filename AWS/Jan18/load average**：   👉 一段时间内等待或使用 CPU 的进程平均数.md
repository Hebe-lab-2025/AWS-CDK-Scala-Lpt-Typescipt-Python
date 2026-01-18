Standby 是一个已同步但处于非活动状态的备用实例，在发生故障时会自动接管。

### `aws rds describe-db-instances --db-instance-identifier mysql8`

**翻译：**

> 查询 **DB 实例标识符为 `mysql8` 的 RDS 实例** 的详细信息。

---

### 这条命令在做什么？

* **`aws rds describe-db-instances`**
  👉 调用 RDS API，获取数据库实例的**完整配置与状态**
* **`--db-instance-identifier mysql8`**
  👉 只查询名为 **mysql8** 的那一个实例（不加这个参数会列出所有）

---

### 常见你会看到的关键信息

返回是 **JSON**，里面包括（面试/排错高频）：

* `DBInstanceStatus`：实例状态（available / creating / modifying）
* `Endpoint.Address`：RDS 连接地址
* `AvailabilityZone`：主库所在 AZ
* `MultiAZ`：是否开启 Multi-AZ
* `Engine` / `EngineVersion`：MySQL 版本
* `DBInstanceClass`：实例规格（如 db.t4g.micro）
* `BackupRetentionPeriod`：备份保留天数

---

### 常用配合命令（更好读）

```bash
aws rds describe-db-instances \
  --db-instance-identifier mysql8 \
  --query 'DBInstances[0].DBInstanceStatus'
```

```bash
aws rds describe-db-instances \
  --db-instance-identifier mysql8 \
  --query 'DBInstances[0].Endpoint.Address'
```

---

### 一句话面试版

> **This command retrieves detailed configuration and status information for a specific RDS DB instance.**

如果你愿意，我可以帮你把 **describe-db-instances 的 JSON 里“必会读的 8 个字段”**整理成一张速查表。


**load 的中文含义（技术语境）**

### 常见翻译

* **负载**
* **负荷**
* **系统压力**

---

### 在计算机 / AWS 场景中的意思

**load** 指系统当前承受的工作量，例如：

* 请求数量
* 并发连接数
* CPU / 内存使用压力

---

### 常见搭配

* **high load** → 高负载
* **low load** → 低负载
* **handle heavy load** → 处理高负载
* **load balancing** → 负载均衡

---

### Linux 里常见的 load

* **load average**：
  👉 一段时间内等待或使用 CPU 的进程平均数

---

### 一句话面试版

> **Load refers to the amount of work a system is handling at a given time.**


**翻译：**

**或者可用区（Availability Zone），以降低发生故障的风险。**

Availability zone to reduce the risk of failure.


Read replicas in the same availability zone
位于同一可用区（Availability Zone）内的只读副本（Read Replicas）。

如需更自然一点的技术表达，也可以说：

同一可用区中的读副本

reducing latency 的中文翻译：👉 降低延迟
