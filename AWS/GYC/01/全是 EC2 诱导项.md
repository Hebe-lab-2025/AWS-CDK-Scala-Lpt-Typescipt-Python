好，**只给你最有杀伤力的两样**，不加废话👇
目标：**看到 EC2 选项 → 条件反射不选**

---

# 👉②【全是 EC2 诱导项】地狱小卷（专防“手一滑选 EC2”）

**⏱️ 3 秒 / 题｜共 20 题｜只选一个**

> 规则：
>
> * 每题 **至少 2 个 EC2 自建选项**
> * 正确答案 **100% 是托管服务**

---

### Q1

私有子网 EC2 访问 S3，要求 **lowest cost**
A. EC2 + Squid proxy
B. NAT Gateway
C. **S3 Gateway Endpoint**
D. EC2 + cron 同步 S3

---

### Q2

需要 **HTTP 路径路由 + TLS 终止**
A. EC2 + Nginx
B. EC2 + HAProxy
C. **ALB**
D. NLB

---

### Q3

Web 层突发流量，耗时任务不能阻塞请求
A. EC2 后台线程
B. EC2 cron 扫任务
C. **SQS + worker（Lambda/ASG）**
D. 同步调用下游

---

### Q4

需要 **fan-out** 给多个系统
A. EC2 写多个 webhook
B. EC2 转发消息
C. **SNS（→ 多 SQS）**
D. SQS 单队列

---

### Q5

RDS 要 **automatic failover**
A. EC2 自建 MySQL 主从
B. Read Replica
C. **RDS Multi-AZ**
D. EC2 定时备份

---

### Q6

题干出现 **sub-millisecond latency / hot keys**
A. EC2 本地 HashMap 缓存
B. 更大 RDS
C. **ElastiCache / DAX**
D. Aurora Global

---

### Q7

需要 **TCP + static IP**
A. EC2 + LVS
B. **NLB**
C. ALB
D. EC2 + Nginx stream

---

### Q8

私有子网拉 ECR 镜像失败（无公网）
A. EC2 NAT Instance
B. NAT Gateway
C. **ECR Interface Endpoint**
D. EC2 复制镜像

---

### Q9

API 需要 **鉴权 + 限流 + 最少运维**
A. EC2 自建网关
B. ALB + EC2
C. **API Gateway**
D. Route 53

---

### Q10

需要 **workflow：重试 / 分支 / 人工审批**
A. EC2 脚本
B. SQS
C. **Step Functions**
D. SNS

---

### Q11

同 VPC 不同子网通信失败
A. 换更大 EC2
B. 新建 EC2 中转
C. **检查 SG / NACL / 路由表**
D. VPC Peering

---

### Q12

需要 **WebSocket**
A. EC2 + ws server
B. NLB
C. **ALB**
D. CloudFront only

---

### Q13

需要 **全球静态内容加速 + HTTPS**
A. EC2 CDN
B. ALB
C. **CloudFront + ACM**
D. Route 53

---

### Q14

只允许公司 IP 访问后台
A. EC2 iptables
B. **SG / NACL allowlist（可叠 WAF）**
C. Shield
D. CloudFront

---

### Q15

私有子网免开 22 端口管理实例
A. Bastion EC2
B. EC2 VPN
C. **SSM Session Manager**
D. EIP

---

### Q16

需要 **事件路由规则**（按类型分发）
A. EC2 判断逻辑
B. SNS
C. **EventBridge**
D. SQS

---

### Q17

数据库读压力大
A. 更大 EC2
B. Multi-AZ
C. **Read Replica**
D. EC2 本地缓存文件

---

### Q18

需要 **严格顺序 + 去重**
A. EC2 顺序处理
B. SNS
C. **SQS FIFO**
D. SQS Standard

---

### Q19

需要集中日志检索，最少运维
A. EC2 ELK
B. **CloudWatch Logs / OpenSearch**
C. RDS
D. S3

---

### Q20

题干强调 **managed / low ops / quick to deploy**
A. EC2 自建
B. EC2 + 脚本
C. **托管服务选项**
D. 自己设计架构

---

### ✅ 答案

```
1C 2C 3C 4C 5C
6C 7B 8C 9C 10C
11C 12C 13C 14B 15C
16C 17C 18C 19B 20C
```

---

## 🧠 EC2 反射口诀（**看到就背**）

> **题干写 managed / least ops / scalable / cost-effective**
> 👉 **EC2 = 99% 诱导项**

---

# 👉③【65 题真实节奏】最后 1–2 天专用模拟法（不再刷题）

## ⏱️ 真考节奏（严格执行）

* **0–15 分钟：12 题**
* **15–30 分钟：25 题**
* **30–60 分钟：50 题**
* **60–75 分钟：做完 65 题**
* **75–90 分钟：只回看 10–15 题**

---

## 🔁 回看优先级（只回这三类）

1. **两选一纠结题**（ALB vs NLB / Multi-AZ vs Replica / SNS vs SQS）
2. **绝对词**：`must / only / lowest cost / no internet / sub-ms`
3. **你当时犹豫超过 10 秒的题**

---

## 📌 最后 10 分钟【保命清单】

* **HA ≠ 性能**

  * HA → Multi-AZ
  * 读 → Replica
* **延迟题先想 Cache**
* **fan-out = SNS**
* **削峰 = SQS**
* **HTTP/L7 = ALB；TCP/L4 = NLB**
* **看到 EC2 自建 → 先否掉**

---

### 下一步（只在你需要时）

* 🔥 **再来一套 30 题“只剩两项都像对”的终极卷**
* 🧠 **把你易错点做成 1 页“考前 5 分钟清单”**

你只回一句：**“继续终极两选一”** or **“做考前清单”**
