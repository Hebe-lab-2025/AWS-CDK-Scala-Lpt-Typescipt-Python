这一步在做的事可以一句话概括为：

> **用 EC2 Instance Connect 登录 EC2，在服务器上跑 React，并通过公网 IP:3000 访问。**

我帮你把**发生了什么 + 为什么这样做 + 面试怎么说**一次讲清。

---

## 1️⃣ EC2 Instance Connect 是什么？

![Image](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2023/06/12/ec2-instance-connect-endpoint.drawio-1-742x630.png)

![Image](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2019/06/25/Picture1-2.png)

**本质**

* **AWS 提供的“免 key 的 SSH 登录方式”**
* 通过 **AWS Console / CLI** 临时注入 SSH key

**解决什么问题**

* ❌ 不用下载 `.pem`
* ❌ 不用自己配 SSH key
* ✅ 权限由 **IAM + Security Group** 控制

👉 你现在能连上 EC2，靠的是：

* SG 允许 **22**
* EC2 Instance Connect + IAM

---

## 2️⃣ 你在 EC2 里实际干了什么（很重要）

### ① 安装 Git

```bash
sudo dnf -y update
sudo dnf -y install git
```

* EC2 = 一台**裸 Linux 服务器**
* 默认啥都没有，需要你装

---

### ② 安装 Node.js

* 为了跑 React
* Node = React 的运行时

---

### ③ clone 仓库 + install

```bash
git clone ...
npm install
```

* 把代码拉到 **EC2 本地磁盘（EBS）**
* 安装依赖

---

### ④ 启动 React

```bash
npm start
```

这一步发生了什么？

* React dev server 启动
* **监听端口 3000**

---

## 3️⃣ 为什么浏览器能访问 `PublicIP:3000`？

必须 **三个条件同时满足** 👇

### ✅ 1. EC2 有 Public IPv4

* 在 Public Subnet
* 绑定了公网 IP

### ✅ 2. Security Group 放行 3000

```text
Inbound: TCP 3000 → 0.0.0.0/0
```

### ✅ 3. 应用真的在监听 3000

```bash
netstat -tulpn | grep 3000
```

👉 少一个都不行（这是经典排错点）

---

## 4️⃣ 架构层面你现在处于哪一步？

```
Browser
  ↓ HTTP :3000
EC2 instance
  ↓
React App (Node.js)
```

⚠️ 注意：

* 这是 **单机直跑**
* ❌ 没有容器
* ❌ 没有负载均衡
* ❌ 没有自动恢复

👉 **这是教学 / Lab 级别，不是生产架构**

---

## 5️⃣ 官方那段话真正想让你学会什么？

> *“Congratulations on deploying your first application on an EC2 instance.”*

重点不是 React，而是 👇

* EC2 是 **通用计算资源**
* 你要 **自己配置环境**
* 安全完全靠 **Security Group + IAM**
* 伸缩、稳定性要你自己负责

---

## 6️⃣ 面试级一句话总结（直接背）

**中文**

> 我使用 EC2 Instance Connect 安全登录 EC2，在实例上部署并运行了一个 React 应用，并通过配置 Security Group 使其可以通过公网 IP 和端口访问。

**英文**

> I accessed the EC2 instance using EC2 Instance Connect, deployed a React application on it, and exposed it through a public IP and port using security group rules.

---

## 7️⃣ 非常关键的一点（为后面铺垫）

你现在这个状态，会在后续 Lab 被 **“否定”** 👇
然后引出：

* ❓ 每次登录装环境合理吗？
* ❓ 一台 EC2 挂了怎么办？
* ❓ 多用户访问怎么办？

👉 **答案就是：Docker → ECR → ECS / Fargate**

---

如果你愿意，我可以下一步帮你：

* 🧠 用一句话解释 **为什么生产不用这种方式**
* 🔄 把你现在这一步 **无痛迁移到 ECS / Fargate**
* 🧪 出 5 道 **EC2 Instance Connect 面试题**

你要 **继续 Lab 下一步**，还是 **先吃透这一阶段的意义**？
