下面是一整套 **“网络连不通定位 = 秒杀面试 + 真生产排错”** 的硬核包：
**先做题 → 决策图 → 状态码对照表**（顺序就是面试官脑回路）

---

## 🧪 10 道「**连不通到底是谁的锅**」排错题（高命中）

> 口诀先背：**Route → NACL → SG → Service**

---

### 1️⃣ EC2 能 ping 自己，外部 ping 不通

**是谁的锅？** → **Security Group**
**原因**：SG 默认拒绝 inbound ICMP
**修复**：Inbound 允许 ICMP / TCP

---

### 2️⃣ Private Subnet EC2 `yum install` 失败

**是谁的锅？** → **Route Table / NAT**
**原因**：`0.0.0.0/0` 没指向 NAT
**不是**：SG（新手常错）

---

### 3️⃣ ALB 健康检查失败（Target unhealthy）

**是谁的锅？** → **Security Group（Backend）**
**原因**：EC2 SG 没放行 **ALB SG → 端口**

---

### 4️⃣ 同 VPC 不同 Subnet，互相访问不了

**是谁的锅？** → **NACL**
**原因**：NACL 是 **stateless**，回包没放
**关键点**：Inbound + Outbound 都要开

---

### 5️⃣ EC2 绑了公网 IP，但还是连不上

**是谁的锅？** → **Route Table**
**原因**：Public Subnet 没有
`0.0.0.0/0 → IGW`

---

### 6️⃣ Lambda 放进 VPC 后访问外网失败

**是谁的锅？** → **NAT / Route Table**
**原因**：Lambda ENI 在 Private Subnet
**没有 NAT = 没网**

---

### 7️⃣ ALB 返回 502

**是谁的锅？** → **后端服务没活着**
**不是**：ALB 本身
**深挖**：Task crash / 端口不监听

---

### 8️⃣ ALB 返回 504

**是谁的锅？** → **应用层 / 超时**
**原因**：Backend 太慢 / DB 卡死
**不是**：网络 ACL

---

### 9️⃣ 访问直接 timeout（连 SYN 都没回）

**是谁的锅？** → **NACL / Route**
**判断**：连不上 ≠ 被拒绝

---

### 🔟 一切都对，还是不通

**终极排查顺序（面试金句）**：

```text
DNS → Route → NACL → SG → Service Port
```

---

## 🧠 SG / NACL / Route Table「**联合决策图**」

![Image](https://4sysops.com/wp-content/uploads/2016/09/NACL-vs.-Security-Group-diagram.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2Ar563mL_PJHbMTczdlwf2rA.png)

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/vpc-bpa-2.png)

```text
🚦 一次请求的真实路径

Internet
   ↓
Route Table（能不能走？）
   ↓
NACL（方向是否都放行？）
   ↓
Security Group（是否允许这个源+端口？）
   ↓
Service（端口是否监听？进程是否活着？）
```

### 三兄弟一句话区分（必背）

| 组件                 | 作用      | 关键特性          |
| ------------------ | ------- | ------------- |
| **Route Table**    | 决定“往哪走” | 没路 = 直接丢      |
| **NACL**           | 子网级防火墙  | **Stateless** |
| **Security Group** | 实例级防火墙  | **Stateful**  |

---

## 🔁 ALB 502 / 504 / 403「**一眼定位表**」

![Image](https://miro.medium.com/1%2AgMTmNn6hAroMM0ykboqLxQ.png)

![Image](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2019/10/06/illustration-2-779x630.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A3VAzlxfIw2T15EIB7ss-Zg.png)

### 🚨 状态码 → 出问题的层

| 错误码                         | 问题层级                         | 真实含义             |
| --------------------------- | ---------------------------- | ---------------- |
| **502 Bad Gateway**         | **Backend / 容器**             | 端口没监听 / Task 崩了  |
| **504 Gateway Timeout**     | **应用 / DB**                  | 后端太慢             |
| **403 Forbidden**           | **SG / WAF / Listener Rule** | 被明确拒绝            |
| **503 Service Unavailable** | **容量 / 健康检查**                | 没 healthy target |

---

### 🎯 面试官最爱追问 & 标准答法

**Q：502 和 504 最大区别？**

> 502 = **连上了，但后端不对**
> 504 = **连上了，但等太久**

---

**Q：403 是网络问题吗？**

> 不是，是 **策略拒绝**（SG / WAF / Rule）

---

## 🧠 30 秒「排错总公式」（白板版）

```text
❌ 连不上
→ 看 Route（有没有路）
→ 看 NACL（回包在不在）
→ 看 SG（端口+来源）
→ 看 Service（监听+存活）
```

---

## ✅ 面试收尾金句（直接用）

> “我排网络问题一定先看 Route，其次 NACL，再看 SG，
> 如果都对，那一定是应用层或者端口问题。”

---

如果你愿意，我可以继续帮你：

* 🧪 **20 道真实 AWS 生产事故排错题（ALB / ECS / Lambda）**
* 🧠 **把这套排错流程压成一张 A4 面试白板图**
* 🔄 **把这些题改成 Amazon L5/L6 深挖版追问**

你想走 **刷题** 还是 **白板速背**？
