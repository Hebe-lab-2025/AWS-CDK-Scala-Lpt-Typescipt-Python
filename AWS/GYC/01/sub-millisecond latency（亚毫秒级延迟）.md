
## ⚡ 第二组暗号：Sub-millisecond latency

### 原文含义
**sub-millisecond latency（亚毫秒级延迟）**

### 考试翻译
👉 **“必须非常非常快”**

---

### 正确联想 ✅
**✔ 缓存层 / 内存级访问：**
- Amazon ElastiCache（Redis / Memcached）
- DynamoDB Accelerator（DAX）

---

### 错误联想 ❌（直接扣分）
- ❌ 直接查 RDS  
- ❌ 直接查 DynamoDB（不加 DAX）  
- ❌ 复杂的自建缓存方案  

📌 **“亚毫秒” = 你不该直接打数据库**

---

### 🧠 为什么很多人会做错？
**原因：Overthinking（过度设计）**

题目已经暗示了解法，但考生常常会：
- 自己设计复杂架构  
- 用 EC2 + 自建缓存  
- 写一堆脚本和中间层  
- 忽略 AWS 已经提供的托管方案  

👉 **这是 AWS 考试最常见的失分点之一**

---

### 🧭 正确的考试思维模型
不要问：
> “我能不能自己搭一个系统？”

要问：
> **“AWS 有没有已经帮我把这件事做完的托管服务？”**

---

### 🧠 高频「暗号 → 服务」速查表

| 题目关键词 | 真实含义 | 直接想到 |
|-----------|----------|----------|
| minimal ops | 不想管运维 | Lambda / S3 / DynamoDB |
| no servers | 无服务器 | Lambda |
| highly scalable | 自动扩展 | DynamoDB / S3 |
| sub-ms latency | 极低延迟 | ElastiCache / DAX |
| burst traffic | 突发流量 | Lambda / SQS |
| cost-effective | 少花钱 | Serverless |
| no admin | 不想维护 | Managed Service |

---

### 🧠 一句话总结（考试必记）
> **AWS 考试不是考你会不会造轮子，而是考你会不会「不造轮子」。**

---

### 👉 下一步（可选）
- 🔹 整理一整页 **AWS 考试「暗号词典」**
- 🔹 出 **10 道关键词秒杀题** 训练直觉
- 🔹 做一张 **考前速记卡**
- 🔹 用真实错题拆解：**为什么 EC2 是错的**

