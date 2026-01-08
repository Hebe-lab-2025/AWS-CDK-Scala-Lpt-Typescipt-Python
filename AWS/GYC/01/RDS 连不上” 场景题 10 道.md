好，这一轮我们 **只盯数据库（RDS）**，完全按 **SAA-C03 出题思维** 来。
你可以把下面内容当成 **“RDS 必杀包”**。

---

# 1️⃣ 专练：**“RDS 连不上” 场景题 10 道（真题口径）**

> 核心考试思维：
> **数据库连不上 ≠ 数据库坏了**
> 👉 先查 **网络 / 权限 / 路径**

---

### Q1

应用在 **公有子网 EC2**，RDS 在 **私有子网**，连接超时。最可能原因？

A. RDS 实例太小
B. RDS 没开 Multi-AZ
C. Security Group 没放行
D. 没有 Read Replica

✅ **C**

**秒杀点**

* EC2 → RDS：**必须放行 SG 入站规则**

---

### Q2

EC2 在私有子网，RDS 在私有子网，同一个 VPC，但连不上。

A. 需要 NAT Gateway
B. NACL 阻断
C. 需要 Internet Gateway
D. 需要 Read Replica

✅ **B**

**秒杀点**

* 同 VPC 私网通信 **不需要 NAT / IGW**
* 重点查 **NACL / SG**

---

### Q3

Lambda 访问 RDS，连接失败。

A. Lambda 不支持访问数据库
B. Lambda 没配置 VPC
C. RDS 不是 Multi-AZ
D. 数据库太慢

✅ **B**

**秒杀点**

* Lambda 默认 **不在 VPC 内**
* 访问 RDS ⇒ **必须配置 VPC + Subnet + SG**

---

### Q4

RDS 新建后，应用立刻连不上。

A. 数据库冷启动
B. DNS 缓存
C. 状态不是 Available
D. 需要 Read Replica

✅ **C**

**秒杀点**

* RDS 创建/修改期间：**不可连接**

---

### Q5

连接数偶发性打满，应用报错 “too many connections”。

A. 加 Read Replica
B. 升级实例规格
C. 使用 RDS Proxy
D. 打开 Multi-AZ

✅ **C**

**秒杀点**

* **连接风暴 = RDS Proxy**

---

### Q6

应用部署在 **另一个 VPC**，RDS 连不上。

A. RDS 不支持跨 VPC
B. 需要 VPC Peering / TGW
C. 需要 NAT Gateway
D. 开启 Multi-AZ

✅ **B**

---

### Q7

应用在 on-premise，VPN 已通，但 RDS 仍连不上。

A. RDS 只允许 AWS 内部
B. SG 未允许 on-prem CIDR
C. RDS 必须公网
D. Read Replica 缺失

✅ **B**

---

### Q8

RDS 公网可访问，但 EC2 仍连不上。

A. 端口错误
B. 实例太小
C. 没开 Multi-AZ
D. AZ 冲突

✅ **A**

---

### Q9

修改 SG 后仍连不上。

A. SG 修改有延迟
B. 需要重启 RDS
C. 查错了 SG（连到错实例）
D. 需要 Read Replica

✅ **C**

---

### Q10

数据库 failover 后，应用短暂断连。

A. 架构错误
B. 正常现象
C. 没用 Read Replica
D. 应该用 DynamoDB

✅ **B**

**考试关键词**

> “**短暂中断是 Multi-AZ 正常行为**”

---

# 2️⃣ **RDS Multi-AZ vs Read Replica 决策图（必背）**

```text
你要解决什么问题？
│
├─ ❓ 高可用 / 容灾 / AZ 故障？
│     └─ ✅ RDS Multi-AZ
│         - 同步复制
│         - 自动 failover
│         - 写入不中断（短暂）
│
├─ ❓ 读压力大 / 性能慢？
│     └─ ✅ Read Replica
│         - 异步复制
│         - 扩展读性能
│         - 不能自动切主
│
├─ ❓ 两个都要？
│     └─ ✅ Multi-AZ + Read Replicas
│
└─ ❌ 成本优化？
      └─ 先想 Read Replica / 实例规格
```

---

# 3️⃣ **RDS 性能 / 扩展 / 成本 高频陷阱题**

### 陷阱 1

> “应用慢，数据库慢”

❌ 错误直觉：升级 RDS
✅ 正确：

* 查 **读/写比例**
* 读多 → Read Replica
* 热点 → ElastiCache

---

### 陷阱 2

> “Multi-AZ 可以提升性能”

❌ 错
✅ Multi-AZ = **可用性，不是性能**

---

### 陷阱 3

> “读副本越多越好”

❌ 错
✅

* 有复制延迟
* 有成本
* 有维护复杂度

---

### 陷阱 4

> “数据库扩展 = 水平扩展”

❌ RDS 主库多是 **纵向扩展**
✅ 水平扩展：

* Read Replica
* Sharding（考试很少）

---

### 陷阱 5

> “高连接数 = 高流量”

❌ 错
✅ 可能是：

* Lambda / 短连接
* 未使用连接池
  👉 **RDS Proxy**

---

# 4️⃣ **SAA-C03 数据库小测（15 题）**

> 只给答案区，适合自测

---

### Q1

高可用写数据库
✅ Multi-AZ

### Q2

读压力大
✅ Read Replica

### Q3

亚毫秒读取
✅ ElastiCache

### Q4

连接数打满
✅ RDS Proxy

### Q5

灾备，不提升性能
✅ Multi-AZ

### Q6

跨区域只读
✅ Cross-Region Read Replica

### Q7

Lambda 访问 RDS
✅ Lambda in VPC

### Q8

私网 EC2 → 私网 RDS
✅ SG / NACL

### Q9

数据库 failover 自动
✅ Multi-AZ

### Q10

数据库 cost 爆炸
✅ 合理规格 + Read Replica

### Q11

只读查询多
✅ Read Replica

### Q12

偶发连接失败
✅ Proxy / Pooling

### Q13

写延迟不能接受
✅ 避免跨 Region 写

### Q14

数据库连不上
✅ 先查网络，不查规格

### Q15

考试看到 “HA”
✅ Multi-AZ

---

## 🧠 一句话考试口诀（送你）

> **连不上 → 先网络
> 慢 → 先读
> HA → Multi-AZ
> 连接爆 → Proxy
> 亚毫秒 → Cache**

---

如果你愿意，下一步我可以直接帮你：

* 🔥 出 **10 道“数据库 + 网络混合坑题”**
* 🧭 做一张 **“数据库题三步排查速记卡”**
* 🧪 来一轮 **完整 SAA-C03 数据库模拟卷（65 题）**

你挑一个继续。
