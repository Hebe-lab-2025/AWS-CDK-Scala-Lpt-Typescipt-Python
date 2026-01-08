下面给你一套 **「RDS 连接 & 性能 实战 + 考试」完整包**，全部按 **SAA-C03 / 面试可直接复述** 的方式来。

---

## 一、RDS 连接是否能打通 —— **决策树（ASCII）**

```
Start
 |
 |-- ① App 和 RDS 在同一个 VPC？
 |        |-- NO → ❌ 无法连接（除非 VPC Peering / TGW）
 |        |-- YES
 |
 |-- ② RDS 在 Private Subnet？
 |        |-- NO → ⚠️ 架构不安全（但不一定连不上）
 |        |-- YES
 |
 |-- ③ RDS Security Group 允许 App？
 |        |-- NO → ❌ Connection refused / timeout
 |        |-- YES
 |
 |-- ④ 端口是否正确？
 |        |-- NO → ❌ Connection refused
 |        |-- YES (MySQL = 3306)
 |
 |-- ⑤ NACL 是否放行？
 |        |-- NO → ❌ Timeout
 |        |-- YES
 |
 |-- ⑥ DNS / Endpoint 是否正确？
 |        |-- NO → ❌ Timeout
 |        |-- YES
 |
 |-- ⑦ 数据库状态是否 available？
 |        |-- NO → ❌ Timeout
 |        |-- YES
 |
 |--> ✅ 连接成功
```

📌 **考试金句**

> If an application cannot connect to RDS, first check Security Groups, then NACLs, then routing.

---

## 二、5 道 AWS 考试风格练习题（含秒杀解析）

### 🧪 Q1

**EC2 无法连接 MySQL RDS，最可能的原因是？**

A. RDS 在 Private Subnet
B. **RDS SG 未允许 EC2 SG** ✅
C. 没有 NAT Gateway
D. 没有 Internet Gateway

📌 **解析**：RDS 不需要 NAT/IGW，只需要 **SG 放行**

---

### 🧪 Q2

**以下哪种情况一定会导致 RDS 连接超时？**

A. 密码错误
B. **NACL 阻止返回流量** ✅
C. 端口写错
D. 用户权限不足

📌 **关键词**：Timeout = 网络被挡

---

### 🧪 Q3

**MySQL RDS 推荐放在哪？**

A. Public Subnet
B. **Private Subnet** ✅
C. 任意 Subnet
D. Default VPC

---

### 🧪 Q4

**RDS 安全组入站规则的最佳实践是？**

A. 允许 0.0.0.0/0
B. 允许 App IP
C. **允许 App Security Group** ✅
D. 不需要入站规则

---

### 🧪 Q5

**下列哪个组件对 RDS 连接最“无感”？**

A. Route Table
B. NACL
C. **NAT Gateway** ✅
D. Security Group

📌 **解析**：RDS 不主动出网，NAT 无关

---

## 三、Amazon RDS

### MySQL RDS **CPU 100% 排查指南（面试 + 实战）**

> 先记住一句话：
> **CPU 100% ≠ 一定是流量大**

---

### Step 1️⃣ 看 CloudWatch 指标（先定位方向）

重点看：

* `CPUUtilization`
* `DatabaseConnections`
* `ReadIOPS / WriteIOPS`
* `FreeableMemory`
* `ReplicaLag`（如果有）

📌 判断逻辑：

* CPU 高 + 连接数高 → **连接风暴**
* CPU 高 + IOPS 高 → **慢 SQL / 全表扫描**
* CPU 高 + 内存低 → **Buffer Pool 不够**

---

### Step 2️⃣ 常见 5 大原因（考试高频）

#### 1. 慢 SQL / 缺索引（🔥 最常见）

* 全表扫描
* `ORDER BY / GROUP BY` 无索引
* `LIKE %xxx`

✅ 解决：

* 打开 **slow query log**
* 加索引 / 重写 SQL

---

#### 2. 连接数过多（连接池没配好）

* App 每个请求新建连接
* 连接未释放

症状：

* `DatabaseConnections` 飙升
* CPU 被线程调度吃满

✅ 解决：

* 使用连接池（HikariCP）
* 限制 max connections

---

#### 3. 单实例规格太小

* db.t3.small 扛高并发
* CPU Credit 用完（Burst）

症状：

* CPU 一直 100%
* 响应越来越慢

✅ 解决：

* 升级实例规格
* 换 non-burst 实例（m / r）

---

#### 4. 读压力集中在主库

* 所有 SELECT 打主库

✅ 解决：

* **Read Replica 分担读**
* 应用读写分离

---

#### 5. 锁竞争（事务太重）

* 长事务
* 行锁 / 表锁

症状：

* CPU 高，但 QPS 不高

✅ 解决：

* 缩短事务
* 合理隔离级别

---

### 🎯 面试总结句（直接说）

> When MySQL RDS CPU hits 100%, I first check CloudWatch metrics to distinguish between slow queries, connection storms, or instance size limits, then mitigate with indexing, connection pooling, or read replicas.

---

## 四、RDS **连接超时 vs 连接拒绝**（考试必考区分）

| 对比    | Connection Timeout          | Connection Refused     |
| ----- | --------------------------- | ---------------------- |
| 含义    | 根本“到不了”                     | 到了，但被拒                 |
| 常见原因  | SG / NACL / Route / DB down | 端口错 / DB 未监听           |
| 网络层   | ❌ 被挡                        | ✅ 通                    |
| 考试关键词 | **Network issue**           | **Service/Port issue** |

---

### 📌 秒选口诀

```
Timeout = 网络不通
Refused = 端口/服务问题
```

---

## 五、终极考试一句话（背这个）

> **RDS connection issues are most commonly caused by Security Group rules, while CPU saturation is typically due to slow queries, excessive connections, or insufficient instance size.**

---

### 下一步我可以继续带你练（直接回数字）👇

1️⃣ 专练 **“RDS 连不上” 场景题 10 道**
2️⃣ 画 **RDS Multi-AZ + Read Replica** 决策图
3️⃣ 出 **RDS 性能 / 扩展 / 成本** 陷阱题
4️⃣ 模拟一轮 **SAA-C03 数据库小测（15 题）**

你选哪个？
