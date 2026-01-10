AWS 考试 = 托管 + 高可用 + 异步 + 最小改动 + 成本合理

下面这张表就是你要的——**《SAA-C03 考前 10 分钟 · 绝对不该选的选项清单》**
目标只有一个：**看到这些选项，条件反射直接排除。**

---

# 🚫 SAA-C03｜绝对不该选的选项清单（10 分钟速记）

## ① 一切 **“自建 / 自管理”**（除非题目强制）

**看到就警惕：**

* EC2 自建 Queue / Cache / DB / LB
* 手写 Failover / Cron / Backup Script
* 自建 Kafka / Redis / Nginx

**为什么不选：**

> AWS 考试默认立场：**托管 > 自建**

**秒杀口诀：**
👉 *“AWS already has this as a managed service”*

---

## ② **单点架构**（题目出现 HA / Fault-tolerant）

**绝对不选：**

* Single EC2
* Single-AZ RDS
* 没有 ASG / 没有 Multi-AZ

**秒杀口诀：**
👉 *“High availability ≠ bigger instance”*

---

## ③ **用性能方案解决可用性问题**

**典型错误：**

* 用 Read Replica 解决故障切换
* 用 Cache 解决 DB 挂掉
* 用更大实例解决宕机

**正确映射：**

* HA → **Multi-AZ**
* 性能 → **Read Replica / Cache**

---

## ④ **NAT Gateway 访问 AWS 服务（能用 Endpoint 却不用）**

**看到就排：**

* “Private subnet access S3/DynamoDB via NAT”

**正确答案：**

* **VPC Endpoint**

  * S3 / DynamoDB → Gateway Endpoint
  * 其他服务 → Interface Endpoint

**秒杀口诀：**
👉 *“Private + AWS service = Endpoint, not NAT”*

---

## ⑤ **RDS 直扛高并发 / 亚毫秒**

**不该选：**

* RDS 直接 serve 高频读
* DynamoDB（无 DAX）却要求 sub-ms

**正确反射：**

* sub-millisecond → **ElastiCache / DAX**

---

## ⑥ **同步做重任务（PDF / Image / Video）**

**错误选项：**

* Web server 同步处理
* 用户请求等待任务完成

**正确姿势：**

* **SQS / Async Worker**

**秒杀口诀：**
👉 *“Heavy task + spikes = async”*

---

## ⑦ **“为了安全”却增加复杂度**

**陷阱选项：**

* 自建加密系统
* 自写 auth / token rotation

**正确反射：**

* Encryption → KMS / IAM
* Temporary access → IAM Role / STS

---

## ⑧ **成本题里选“最强方案”**

**典型误选：**

* Aurora Global
* NLB / Dedicated Host
* 全程 Multi-AZ + Cross-Region

**秒杀口诀：**
👉 *“Cost-effective ≠ best possible”*

---

## ⑨ **忽略“最小改动”原则**

**错误方向：**

* 推倒重来
* 大规模迁移
* 重构架构

**正确方向：**

* 最小改动 + 托管增强

---

## ⑩ **选项里出现 “manual / custom / script”**

**几乎必错关键词：**

* manual failover
* custom script
* self-managed

**秒杀口诀：**
👉 *“Manual = exam poison”*

---

# 🧠 最终保命总口诀（记住这一句就够）

> **AWS 考试 = 托管 + 高可用 + 异步 + 最小改动 + 成本合理**

---

## 如果你愿意，我可以立刻继续👇

* 🔥 把这张清单做成 **手机锁屏 / PDF**
* 🧪 出 **20 道“只考这些错误选项”的反射题**
* 🎯 把它并入你的 **90 分钟双陷阱模考**

你只要说一句：**“继续”**。
