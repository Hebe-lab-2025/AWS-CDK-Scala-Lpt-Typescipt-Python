这两个词在 AWS 里**几乎只和“网络流量方向”有关**，尤其常见在 **Security Group / NACL** 里。

---

## 🧠 一句话结论（先背）

> **Inbound = 进来（谁能访问我）**
> **Outbound = 出去（我能访问谁）**

---

## 🔽 Inbound（入站流量）

### 是什么？

**外部 → 你的资源** 的流量

### 常见例子

* 用户访问你的网站
* 浏览器访问 EC2 / ECS / ALB
* SSH 登录服务器（22）

### AWS 里用在哪？

* **AWS Security Group** Inbound rules
* **AWS Network ACL** Inbound rules

### 示例

```text
Inbound:
允许 TCP 80
来源：0.0.0.0/0
```

👉 **任何人都能访问你的网站**

---

## 🔼 Outbound（出站流量）

### 是什么？

**你的资源 → 外部** 的流量

### 常见例子

* 服务器访问数据库
* 应用调用第三方 API
* EC2 拉 Docker 镜像（ECR）

### AWS 里用在哪？

* Security Group Outbound rules
* Network ACL Outbound rules

### 示例

```text
Outbound:
允许 TCP 443
目标：0.0.0.0/0
```

👉 **服务器可以访问外部 HTTPS 服务**

---

## 🧠 直观流程图

![Image](https://docs.aws.amazon.com/images/prescriptive-guidance/latest/secure-outbound-network-traffic/images/network-firewall-architecture-dns-hostnames.png)

![Image](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/security-group-referencing.png)

```
User ──▶ Inbound ──▶ EC2 / ECS ──▶ Outbound ──▶ Internet / AWS Service
```

---

## 🔐 放在 AWS 网络里怎么理解（很重要）

### Security Group（最常考）

* **Inbound：默认拒绝**
* **Outbound：默认全放行**

👉 所以：

* 你必须**手动开 Inbound**
* Outbound 通常不用动

---

## ⚠️ 高频误区

* ❌ 以为 Inbound / Outbound 是“请求 / 响应”
* ❌ 以为只配 Inbound 就够了（Outbound 也可能被限制）
* ❌ 混淆 Security Group 和 NACL（一个是实例级，一个是子网级）

---

## 🎯 面试一句话（标准版）

> *“Inbound rules control traffic entering a resource, while outbound rules control traffic leaving the resource.”*

---

## 🧠 放回你正在学的架构里

```
User
 ↓ (Inbound 80/443)
ALB / ECS
 ↓ (Outbound 443)
ECR / RDS / Third-party API
```

---

如果你愿意，我可以继续帮你：

* 🧪 **出 5 道 Inbound / Outbound 判断题（90% 新手会错）**
* 🔁 **Security Group vs NACL（终极对比）**
* 🧠 **帮你把一个 EC2 / ECS 的安全组规则“面试化”讲一遍**

你选一个，我继续。
