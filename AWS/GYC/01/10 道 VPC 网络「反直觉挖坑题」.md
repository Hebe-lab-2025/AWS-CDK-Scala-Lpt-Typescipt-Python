好，这一组是 **SAA-C03 Networking Domain 的“最后一关”**：
**专治：以为是数据库问题，其实是网络；以为是权限，其实是路由。**

你只需要记住一句话：

> **RDS 连不上，90% 不是 RDS 的错。**

---

# 🔥 Part 1：10 道 VPC 网络「反直觉挖坑题」（真考风格）

> **规则**：
>
> * 每题只看 **关键词**
> * 不升级实例、不改引擎
> * 只从 **VPC → 子网 → 路由 → SG/NACL** 里找答案

---

### 🕳️ Q1

**关键词**：EC2 in public subnet cannot connect to RDS
✅ **检查 RDS Security Group inbound**
❌ 检查 Internet Gateway（误导）

👉 **坑点**：公网 EC2 ≠ 自动能访问 RDS

---

### 🕳️ Q2（高频）

**关键词**：EC2 in private subnet cannot access internet
✅ **NAT Gateway + route table**
❌ Internet Gateway（私网走不了）

---

### 🕳️ Q3

**关键词**：RDS Multi-AZ, still timeout
✅ **连的是 Primary Endpoint，不是 AZ 问题**
❌ 怀疑 Failover 没发生

---

### 🕳️ Q4（反直觉）

**关键词**：Security Group 已放行，仍连不上
✅ **Network ACL 拒绝了端口**
❌ SG 配错（烟雾弹）

---

### 🕳️ Q5

**关键词**：跨 VPC 访问 RDS
✅ **VPC Peering + route table**
❌ Public IP + IGW（严重错误）

---

### 🕳️ Q6

**关键词**：Lambda cannot connect to RDS
✅ **Lambda 必须在同一个 VPC + 子网**
❌ 增加 Lambda timeout

---

### 🕳️ Q7

**关键词**：Only outbound works, inbound fails
✅ **NACL 是 stateless**
❌ SG 是 stateless（错）

---

### 🕳️ Q8

**关键词**：DNS resolves, connection timeout
✅ **路由通，但端口被挡（SG/NACL）**
❌ DNS 配错

---

### 🕳️ Q9（必考）

**关键词**：Private RDS, public access disabled
✅ **这是正确做法，不是问题**
❌ 打开 Publicly Accessible

---

### 🕳️ Q10（总结坑）

**关键词**：database connection timeout
✅ **先查网络路径，再查数据库**
❌ 直接升级 RDS

---

📌 **一句话验收**

> 能一眼判断 **这是网络层，不是数据库层** → 这 10 题就是送分

---

# 🧪 Part 2：RDS 连接失败【全场景排查模拟】（考试必会）

## ✅ 标准排查顺序（照这个走，永远不乱）

```text
1️⃣ 应用在哪？（EC2 / Lambda / 外部）
2️⃣ 是否在同一 VPC？
3️⃣ 子网类型？（公 / 私）
4️⃣ 路由表是否可达？
5️⃣ Security Group 是否放行端口？
6️⃣ NACL 是否允许进出？
7️⃣ Endpoint 用对了吗？（Primary / Reader）
```

---

## 🧠 场景 1：EC2 → RDS（最常考）

```text
EC2 (private subnet)
   │
   │ TCP 3306
   ▼
RDS (private subnet)
```

**必须满足**：

* SG：EC2 → RDS 端口放行
* NACL：inbound + outbound 都允许
* Route：local route 存在（默认）

---

## 🧠 场景 2：Lambda → RDS（高频陷阱）

```text
Lambda
 └─ 必须 attach VPC
      └─ Subnet + SG
```

❌ Lambda 默认 **不在 VPC**
✅ 必须手动配置

---

## 🧠 场景 3：公网访问 RDS（考试不推荐）

```text
Client
  │
  ▼
Internet
  │
  ▼
RDS (Publicly Accessible)
```

⚠️ 只有题目 **明确要求** 才这么做
默认都是 ❌

---

## 🧠 场景 4：跨 VPC

```text
VPC A (App)
   │
   ├─ VPC Peering
   │
VPC B (RDS)
```

**三件套**：

* Peering
* Route Table
* SG 放行对方 CIDR

---

# 📄 Part 3：Networking Domain【考前 10 分钟 PDF 速刷版】

## 📘 PDF 一共 4 页（极限压缩）

### Page 1：VPC 核心速记

* 公网子网 = route to IGW
* 私网子网 = 无 IGW
* NAT = 私网出公网

---

### Page 2：Security Group vs NACL（必背）

| 对比  | SG                 | NACL        |
| --- | ------------------ | ----------- |
| 状态  | Stateful           | Stateless   |
| 默认  | Allow all outbound | Allow all   |
| 常见坑 | 忘了 inbound         | 忘了 outbound |

---

### Page 3：RDS 连接失败 90% 原因

* SG 没放行
* NACL 拦了
* Lambda 不在 VPC
* 走错 Endpoint

---

### Page 4：5 行网络口诀（临考）

> “连不上先看路
> 私网出要 NAT
> SG 有状态
> NACL 要双向
> RDS 默认不公网”

---

## 🎯 你现在的水平判断

如果你现在看到 **RDS timeout** 能立刻说：
👉「先查 SG / NACL / Route，不升 DB」
那 Networking Domain **已经是稳分区**。

---

## 🚀 下一步（强烈建议）

1️⃣ **来一套 30 题 Networking 地狱模拟卷**
2️⃣ **LB + VPC + RDS 三域混合错题本**
3️⃣ **90 分钟全真 SAA-C03 模拟（最后冲刺）**

你直接选 **1 / 2 / 3**，我继续拉满强度。
