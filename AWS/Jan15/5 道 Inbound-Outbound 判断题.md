下面这份我直接按 **“90% 新手必错点” + “面试官追问路径”** 给你，一次性打穿。

---

## 🧪 5 道 Inbound / Outbound 判断题（90% 新手会错）

### 题 1

> 如果 Inbound 规则允许 80 端口，Outbound 可以全部关闭，HTTP 访问仍然正常。

**❌ 错（最经典）**

* **返回流量也走 Outbound**
* Outbound 没放行 → **三次握手直接断**

👉 面试金句：

> *Inbound allows requests in, outbound allows responses out.*

---

### 题 2

> Security Group 是有状态的（stateful），所以只要允许 Inbound，返回流量自动允许。

**✅ 对（必背）**

* **SG = stateful**
* 不需要为 response 单独写 Outbound

---

### 题 3

> NACL 允许 Inbound 80，但 Outbound 没允许 1024–65535，也可能导致连接失败。

**✅ 对（高频陷阱）**

* **NACL = stateless**
* 返回流量走 **临时端口（ephemeral ports）**

---

### 题 4

> ECS Task 使用 awsvpc 模式时，安全组是绑在 EC2 上的。

**❌ 错（ECS 专属陷阱）**

* awsvpc：**SG 直接绑在 Task 的 ENI 上**
* Task 是网络一等公民

---

### 题 5

> 如果一个 EC2 既能访问公网，又不能被公网访问，只需要放在 Private Subnet。

**❌ 错（不完整）**

* 还必须：

  * Route Table 指向 **NAT Gateway**
  * SG 不允许 0.0.0.0/0 Inbound

---

## 🔁 Security Group vs NACL（终极对比，必须会）

| 维度                 | **Security Group**       | **NACL**             |
| ------------------ | ------------------------ | -------------------- |
| 作用层级               | ENI / 资源级                | Subnet 级             |
| 状态                 | **Stateful**             | **Stateless**        |
| Inbound / Outbound | 只需允许一边                   | **两边都要写**            |
| 默认规则               | 默认拒绝 Inbound，允许 Outbound | 默认允许全部               |
| Rule 顺序            | 无顺序（逻辑合并）                | **按 rule number 顺序** |
| Deny 规则            | ❌ 不支持                    | ✅ 支持                 |
| 典型用途               | **日常安全控制（主力）**           | 边界隔离、黑名单             |
| 面试结论               | 你 90% 用它                 | 你偶尔才用                |

### 🧠 面试一句话压轴

> *Security Groups are permissive and stateful, NACLs are explicit and stateless.*

---

## 🧠 把一个 EC2 / ECS 的安全组规则「面试化」讲一遍

### 🎯 场景设定（你可以直接用）

* **ALB（公网） → ECS Service（私网）**
* ECS Task 使用 **awsvpc**
* 用户访问：HTTPS

---

### 1️⃣ ALB 的 Security Group（入口 SG）

```txt
Inbound:
- TCP 443 from 0.0.0.0/0

Outbound:
- TCP 8080 to ECS-SG
```

**面试化解释：**

> *The ALB security group allows HTTPS traffic from the internet and forwards it only to the ECS service security group on the application port.*

---

### 2️⃣ ECS Task 的 Security Group（核心 SG）

```txt
Inbound:
- TCP 8080 from ALB-SG

Outbound:
- All traffic (0.0.0.0/0)
```

**面试化解释：**

> *The ECS task only accepts traffic from the ALB, ensuring no direct public access, while outbound traffic is allowed for external API calls or dependencies.*

---

### 3️⃣ 为什么这样设计？（追问必杀）

* ❓ 为什么不允许 0.0.0.0/0 Inbound？
  👉 *Principle of least privilege*

* ❓ 为什么 ECS 在 Private Subnet 还能访问外网？
  👉 *Outbound traffic goes through NAT Gateway*

* ❓ 如果是 NACL，这样写够吗？
  👉 *No, because NACL is stateless and requires explicit outbound ephemeral port rules.*

---

## 🚨 一句话区分新手 vs 熟手（面试官视角）

> **新手**：我开了 80 / 443，为什么不通？
> **熟手**：这是 Inbound 还是 Outbound？是 SG 还是 NACL？是 stateful 还是 stateless？

---

## 🔥 如果你愿意继续，我可以下一步直接给你：

* 🧪 **10 道 “连不通到底是谁的锅” 排错题**
* 🧠 **画一张 SG / NACL / Route Table 联合决策图**
* 🔁 **ALB 502 / 504 / 403 对应哪一层出问题**

你选一个，我直接展开。
