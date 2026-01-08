下面给你一整套 **SAA-C03 Load Balancer 决胜包（可直接考前刷）**，全部是**高频陷阱 + 秒选逻辑**，不讲废话。

---

# ✅ 一、20 道 ELB + Auto Scaling 联动陷阱题（含秒杀点）

### Q1

**场景**：流量突增，实例自动扩容
**问**：新 EC2 能否自动接流量？

* A. 需要手动注册到 ELB
* B. Auto Scaling 自动注册到 ELB ✅

👉 **暗号**：ASG + ELB = 自动注册

---

### Q2

**场景**：扩容后用户会话丢失
**正确方案**：

* ❌ Sticky Session（治标）
* ✅ Stateless + Session 存 Redis / DynamoDB

👉 **暗号**：ASG = 无状态设计

---

### Q3

**场景**：实例不健康但仍有请求
**原因**：

* ❌ ASG 失效
* ✅ Health Check 未与 ALB 同步

👉 **暗号**：ALB health check ≠ EC2 status check

---

### Q4

**场景**：扩容慢，用户超时
**最优解**：

* ✅ ALB + ASG + Warm Pool

👉 **暗号**：scale fast = Warm Pool

---

### Q5

**场景**：实例 CPU 低但请求慢
**真正瓶颈**：

* ❌ 扩容实例
* ✅ 下游 DB / 网络 / 连接数

👉 **暗号**：低 CPU ≠ 性能好

---

### Q6

**场景**：突然缩容，连接中断
**原因**：

* ❌ ASG bug
* ✅ Deregistration delay 太短

👉 **暗号**：连接不中断 = 延迟下线

---

### Q7

**场景**：某 AZ 故障
**系统是否可用？**

* ✅ ALB + ASG 跨 AZ 自动恢复

👉 **暗号**：multi-AZ 是默认高可用

---

### Q8

**场景**：实例启动慢
**最优扩容策略**：

* ❌ 增大 instance
* ✅ 提前 scale + 预测型扩容

👉 **暗号**：predictive scaling

---

### Q9

**场景**：新实例不接流量
**检查顺序**：
1️⃣ Target Group
2️⃣ Health Check Path
3️⃣ Security Group

👉 **暗号**：LB → TG → SG

---

### Q10

**场景**：HTTPS 在实例上处理
**更优方案**：

* ✅ TLS 终止在 ALB

👉 **暗号**：offload SSL

---

### Q11

**场景**：请求只打到一个实例
**原因**：

* ❌ ALB 不均衡
* ✅ Sticky Session 开启

👉 **暗号**：stickiness = 单点风险

---

### Q12

**场景**：扩容但 RDS 扛不住
**正确解法**：

* ✅ Read Replica / Cache

👉 **暗号**：ASG 不解决 DB

---

### Q13

**场景**：健康检查失败但实例正常
**原因**：

* ✅ 应用端口不一致

---

### Q14

**场景**：流量峰值固定
**最佳扩容**：

* ✅ Scheduled Scaling

---

### Q15

**场景**：蓝绿发布
**最佳组合**：

* ✅ ALB + 多 Target Group

---

### Q16

**场景**：实例频繁被替换
**原因**：

* ✅ Health check 太严格

---

### Q17

**场景**：扩容成功但 5xx 上升
**原因**：

* ✅ 下游依赖未扩容

---

### Q18

**场景**：想按 URL 路由流量
**必须选**：

* ✅ ALB

---

### Q19

**场景**：WebSocket 连接
**正确选择**：

* ✅ ALB

---

### Q20

**一句话总结**

> **ASG 解决容量，ALB 解决流量，不解决状态**

---

# ✅ 二、ALB vs CloudFront vs API Gateway（三选一魔鬼题）

| 暗号词                            | 秒选           |
| ------------------------------ | ------------ |
| **Global users / CDN / 静态内容**  | CloudFront   |
| **REST / HTTP API / JWT / 限流** | API Gateway  |
| **EC2 / 容器 / URL 路由**          | ALB          |
| **WebSocket**                  | ALB / API GW |
| **缓存静态图片**                     | CloudFront   |
| **微服务后端入口**                    | ALB          |
| **Serverless API**             | API Gateway  |

---

### 必杀组合题

> 全球用户访问 EC2 Web 应用，要求低延迟

✅ **CloudFront + ALB + ASG**

---

# ✅ 三、SAA-C03 Load Balancer 全套 PDF（结构说明）

你这套 PDF 我会按 **可打印 + 速记** 设计：

**目录结构**

1. 一页 ALB / NLB / CLB 对比表
2. ELB + ASG 经典架构图
3. 20 个必错陷阱（就是上面这套）
4. 三选一决策树（ALB / CF / API GW）
5. 考前 1 页「看到暗号就秒选」表

👉 如果你要：

* 📄 **A4 打印版**
* 📱 **手机纵向版**
* 🧠 **Notion / Obsidian 模板**
  我可以直接给你生成。

---

# ✅ 四、30 秒口述版考试速记稿（临考用）

> **一句话背下来即可**

> “EC2 前面默认用 ALB，自动扩容交给 Auto Scaling；
> 全球加速和静态内容用 CloudFront；
> Serverless API 才用 API Gateway；
> ASG 不解决会话，要无状态；
> 性能慢先看下游，不要盲目扩容。”

---

## 🚀 下一步我可以继续帮你：

* 🔥 **再出 30 道「只看关键词秒选」LB 盲选题**
* 🧠 **把 LB + ASG + RDS 串成一页终极决策树**
* 🧪 **90 分钟完整 Load Balancer 模拟小卷**

你说 **继续哪一项**。
