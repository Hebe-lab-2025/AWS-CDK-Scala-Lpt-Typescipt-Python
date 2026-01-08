## SSL 是啥？（一句话先记）

> **SSL = Secure Sockets Layer**
> 👉 **用来给网络通信“加密 + 验证身份”，防止被偷看或篡改**

> 📌 现在**考试 / 实际上用的都是 TLS**，
> 但大家**习惯统称 SSL**。

---

## 一、SSL 到底解决什么问题？

在**没有 SSL（HTTP）**的情况下：

* 密码 👉 **明文**
* Cookie 👉 **明文**
* 请求内容 👉 **明文**
* ⚠️ 路由器 / Wi-Fi / 中间人都能看

加了 **SSL（HTTPS）**之后：

* 🔒 数据被 **加密**
* 🧾 服务器身份被 **验证**
* 🛡️ 数据 **不能被篡改**

![Image](https://www.mariogerard.com/wp-content/uploads/2018/01/TPM-HTTP-vs-HTTPS.png)

![Image](https://cf-assets.www.cloudflare.com/slt3lc6tev37/5aYOr5erfyNBq20X5djTco/3c859532c91f25d961b2884bf521c1eb/tls-ssl-handshake.png)

---

## 二、SSL 做了三件核心事情（考试必背）

### 1️⃣ **加密（Encryption）**

* 把明文 → 密文
* 防止 **偷看**

### 2️⃣ **身份验证（Authentication）**

* 确认你连的是 **真服务器**
* 防止 **钓鱼网站**

### 3️⃣ **完整性校验（Integrity）**

* 确保数据没被改
* 防止 **中间人攻击**

📌 **考试关键词三连击**：

> Encryption + Authentication + Integrity

---

## 三、SSL / TLS 工作流程（简化版）

![Image](https://cf-assets.www.cloudflare.com/slt3lc6tev37/5aYOr5erfyNBq20X5djTco/3c859532c91f25d961b2884bf521c1eb/tls-ssl-handshake.png)

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20220606010323/SSLHandshakeDiagByakhilabhilash01.png)

```
1. Client → Server：我支持这些加密算法
2. Server → Client：证书（含公钥）
3. Client：验证证书 + 生成对称密钥
4. 双方用【对称加密】开始通信
```

📌 **重点记住**：

* **证书 + 非对称** → 建立信任
* **正式通信用对称加密（快）**

---

## 四、SSL 证书是啥？

> **SSL 证书 = 服务器的“身份证”**

证书里包含：

* 域名
* 公钥
* 颁发机构（CA）
* 有效期

常见 CA：

* DigiCert
* Let’s Encrypt
* AWS ACM（考试常见）

---

## 五、在 AWS 里 SSL 用在哪？

### 最典型：**ALB + HTTPS**

![Image](https://miro.medium.com/1%2As7CuGEGGebnJ5RH7Fg163g.png)

![Image](https://docs.aws.amazon.com/images/prescriptive-guidance/latest/patterns/images/pattern-img/cad6e465-da39-4819-970e-10e1c30e0a1f/images/45ac7fba-fbad-4c74-9b1f-80ca212dae08.png)

```
User (HTTPS)
   ↓
ALB（SSL 终止，ACM 证书）
   ↓
EC2 / ECS（HTTP）
```

📌 这叫：**SSL Termination at ALB**

**好处（考试爱考）：**

* 后端不用管证书
* 统一管理
* 性能更好

---

## 六、SSL vs TLS（考试怎么说）

| 项目 | SSL       | TLS   |
| -- | --------- | ----- |
| 状态 | 已淘汰       | 正在使用  |
| 说法 | 习惯叫法      | 正确技术名 |
| 考试 | 说 SSL 也算对 | 👍    |

📌 **一句话**：

> “We use TLS, but people still call it SSL.”

---

## 七、考试常见坑

❌ **HTTP 也能加密** → 错
❌ **NLB 做 HTTPS 内容转发** → 错
❌ **证书装在 EC2 才安全** → 不一定（ALB 更好）

---

## 八、30 秒口试版（考试/面试通用）

> **SSL (now TLS) is a security protocol that encrypts data in transit, verifies server identity using certificates, and ensures data integrity. In AWS, SSL is commonly terminated at ALB using ACM certificates to secure HTTPS traffic.**

---

如果你愿意，我可以继续帮你：

* 🔹 **把 SSL / HTTPS / ALB 串成一张考试流程图**
* 🔹 **讲清“为什么 HTTPS 一定要用对称 + 非对称混合”**
* 🔹 **出 10 道 SSL / HTTPS / ALB 高频考题**
* 🔹 **对比 SSL at ALB vs SSL at EC2**

你想继续 **安全** 还是 **负载均衡**？
