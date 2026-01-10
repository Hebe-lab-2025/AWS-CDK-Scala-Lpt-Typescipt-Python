好，**直接给你“可落地交付版”**，三件事一次完成 👇
（**锁屏 / PDF 内容 + 20 道只考错误选项的反射题 + 并入 90 分钟双陷阱模考**）

---

## 📱 手机锁屏 / PDF【一页暗号清单】

> **尺寸建议**：1080 × 2400（手机锁屏）｜A4（PDF）

```md
# 🔥 AWS SAA-C03 秒选暗号（锁屏版）

## 🔐 IAM / Security
• least privilege → IAM Policy / Role  
• temporary credentials → IAM Role + STS  
• cross-account access → AssumeRole  
• public access blocked → S3 Block Public Access  
• DDoS → Shield (Advanced = L7 + cost)  
• WAF rules → SQLi / XSS

## 🌐 Network
• private subnet + internet → NAT Gateway  
• AWS service private access → VPC Endpoint  
• hybrid DNS → Route 53 Resolver  
• static IP LB → NLB  
• path / host routing → ALB

## ⚙️ Compute
• unpredictable traffic → ALB + ASG  
• no server mgmt → Lambda  
• long-running stateful → EC2  
• containers → ECS / EKS  
• burst → Auto Scaling (not bigger instance)

## 🗄 Database
• HA / failover → RDS Multi-AZ  
• read bottleneck → Read Replica  
• global low latency → DynamoDB Global Table  
• cache read → ElastiCache  
• strong consistency → RDS / Aurora

## 📦 Storage
• static website → S3 + CloudFront  
• cheap archive → Glacier  
• shared POSIX → EFS  
• EC2 local fast → Instance Store

## 📩 Messaging
• decouple async → SQS  
• fan-out → SNS  
• order guarantee → SQS FIFO  
• stream replay → Kinesis

## 💰 Cost Trap
• NAT data cost → VPC Endpoint  
• always-on EC2 → Lambda  
• over-provision → Auto Scaling
```

👉 **你可以直接复制 → 导出 PDF / 设为锁屏**
如果你要，我可以 **直接帮你生成 PDF 下载版**。

---

## 🧪 20 道【只考“错误选项”的反射题】

> **规则**：看到题干 → **秒排除那个“最容易手滑的错项”**

### Q1

**私有子网 EC2 访问 S3**
❌ 错选：NAT Gateway
✅ 正解：**VPC Gateway Endpoint**

---

### Q2

**数据库读慢**
❌ 错选：RDS Multi-AZ
✅ 正解：**Read Replica**

---

### Q3

**需要 HTTPS + 路由规则**
❌ 错选：NLB
✅ 正解：**ALB**

---

### Q4

**不可预测流量 + 最小运维**
❌ 错选：EC2
✅ 正解：**Lambda**

---

### Q5

**对象公开但要防误开**
❌ 错选：Bucket Policy
✅ 正解：**S3 Block Public Access**

---

### Q6

**异步解耦**
❌ 错选：SNS
✅ 正解：**SQS**

---

### Q7

**顺序严格保证**
❌ 错选：Standard SQS
✅ 正解：**FIFO SQS**

---

### Q8

**全局静态 IP LB**
❌ 错选：ALB
✅ 正解：**NLB**

---

### Q9

**Hybrid DNS**
❌ 错选：Public Hosted Zone
✅ 正解：**Route 53 Resolver**

---

### Q10

**高并发短任务**
❌ 错选：ECS
✅ 正解：**Lambda**

---

### Q11

**数据库高可用**
❌ 错选：Read Replica
✅ 正解：**Multi-AZ**

---

### Q12

**EC2 间共享文件**
❌ 错选：S3
✅ 正解：**EFS**

---

### Q13

**防 SQL 注入**
❌ 错选：Shield
✅ 正解：**WAF**

---

### Q14

**AWS 服务私网访问**
❌ 错选：NAT Gateway
✅ 正解：**VPC Endpoint**

---

### Q15

**日志实时分析流**
❌ 错选：SQS
✅ 正解：**Kinesis**

---

### Q16

**减少 NAT 成本**
❌ 错选：缩小 EC2
✅ 正解：**VPC Endpoint**

---

### Q17

**Web 静态加速**
❌ 错选：ALB
✅ 正解：**CloudFront**

---

### Q18

**容器但不想管集群**
❌ 错选：EKS
✅ 正解：**ECS**

---

### Q19

**跨账号访问**
❌ 错选：User Policy
✅ 正解：**AssumeRole**

---

### Q20

**低延迟缓存读**
❌ 错选：Read Replica
✅ 正解：**ElastiCache**

---

## 🎯 并入【90 分钟双陷阱模考】的方式（你直接用）

### 模考结构

* **65 题 / 90 分钟**
* 每题 **2 个陷阱**

  * 陷阱 A：服务相似（ALB vs NLB）
  * 陷阱 B：目标混淆（HA vs 性能）

### 你做题时的 **三步法**

1. **先看暗号**（锁屏那页）
2. **先排错项**（上面 20 类）
3. 再选唯一剩下的

---

## 下一步我可以直接帮你做 👇

* 📄 **生成锁屏尺寸 PDF + A4 PDF**
* 🧠 **把 20 道错项题做成 Anki 闪卡**
* ⏱ **完整 90 分钟双陷阱模拟卷（含错因本）**

👉 你只要回一句：
**「PDF + 模考」 / 「ALL IN」**
