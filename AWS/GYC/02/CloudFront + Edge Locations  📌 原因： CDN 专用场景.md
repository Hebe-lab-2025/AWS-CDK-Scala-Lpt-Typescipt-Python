下面给你一整套 **👉 可直接复制到 GitHub 的 Markdown 内容**，覆盖你要的 **3 个部分**：
**对比图 + 考试陷阱题 + 一页速记卡**，完全按 **AWS 考试（SAA/SOA/DVA）取向**设计。

---

# 🌍 AWS Global Infrastructure 一页速记卡（Exam Ready）

> 目标：**看到关键词，立刻知道是 Region / AZ / Edge Location**

---

## 一、Region / AZ / Edge Location 总对比图（ASCII）

```text
                    🌐 AWS Global Infrastructure

┌─────────────────────────────────────────────────────────┐
│                       AWS Region                        │
│   (Geographic Area, e.g. us-east-1, eu-west-1)          │
│                                                         │
│   ┌───────────────┐   ┌───────────────┐                │
│   │ Availability  │   │ Availability  │                │
│   │ Zone (AZ-a)   │   │ Zone (AZ-b)   │                │
│   │               │   │               │                │
│   │  EC2  RDS     │   │  EC2  RDS     │                │
│   │  ALB  Cache   │   │  ALB  Cache   │                │
│   └───────────────┘   └───────────────┘                │
│          ▲                   ▲                          │
│          │ high-speed, low-latency network              │
└──────────┴───────────────────┴──────────────────────────┘
                     ▲
                     │
        ┌──────────────────────────────────┐
        │          Edge Locations           │
        │ (CloudFront / Route 53 / WAF)     │
        │  • 全球分布                       │
        │  • 靠近用户                       │
        └──────────────────────────────────┘
```

---

## 二、三者核心对比表（必背）

| 维度    | Region          | Availability Zone (AZ) | Edge Location        |
| ----- | --------------- | ---------------------- | -------------------- |
| 本质    | 地理区域            | 数据中心                   | CDN 节点               |
| 是否隔离  | 与其他 Region 完全隔离 | AZ 之间物理隔离              | 不运行主业务               |
| 距离用户  | 较远              | 较远                     | **最近**               |
| 主要作用  | 资源部署            | 高可用                    | **低延迟内容分发**          |
| 典型服务  | EC2, S3, RDS    | Multi-AZ, ALB          | CloudFront, Route 53 |
| 考试关键词 | geographic      | fault isolation        | **low latency**      |

---

## 三、🧠 一页「关键词 → 秒选」反射表（核心）

| 题目关键词                        | 秒选                |
| ---------------------------- | ----------------- |
| **geographic location**      | Region            |
| **fault tolerance / HA**     | Multiple AZs      |
| **data center failure**      | AZ                |
| **lowest latency for users** | Edge Location     |
| **static content delivery**  | CloudFront (Edge) |
| **global users**             | Edge Location     |
| **disaster recovery**        | Multi-Region      |
| **high availability**        | Multi-AZ          |

---

## 四、🧪 Edge Location 高频考试陷阱题（5 道）

---

### ❌ 陷阱题 1

**题目**：

> Deploy your application closer to end users to reduce latency.

❌ 错误选项：

* Deploy EC2 instances in multiple AZs

✅ 正确答案：
👉 **Use CloudFront with Edge Locations**

📌 原因：

* AZ ≠ 靠近用户
* Edge 才是用户最近

---

### ❌ 陷阱题 2

**题目**：

> Edge Locations are used to host EC2 instances.

❌ 错误理解：

* Edge = 小型 Region

✅ 正确认知：
👉 **Edge Locations do NOT run EC2**

📌 原因：

* Edge 只用于内容分发 / DNS / 安全

---

### ❌ 陷阱题 3

**题目**：

> Improve availability within a single region.

❌ 错误选项：

* Use Edge Locations

✅ 正确答案：
👉 **Use multiple Availability Zones**

📌 原因：

* HA = AZ
* Edge = latency

---

### ❌ 陷阱题 4

**题目**：

> Serve static images globally with minimal latency.

❌ 错误选项：

* Replicate EC2 instances in multiple regions

✅ 正确答案：
👉 **CloudFront + Edge Locations**

📌 原因：

* CDN 专用场景

---

### ❌ 陷阱题 5

**题目**：

> Protect against an AZ failure.

❌ 错误选项：

* Use Edge Locations

✅ 正确答案：
👉 **Deploy resources across multiple AZs**

📌 原因：

* Edge 不是 HA 方案

---

## 五、AWS Global Infrastructure 考试“反直觉”规则（必背）

```text
❗ Edge ≠ Region
❗ Edge 不跑 EC2 / RDS
❗ HA = Multi-AZ
❗ DR = Multi-Region
❗ Low latency = Edge Location
```

---

## 六、30 秒考前脑内检查清单

* 问的是 **延迟** 还是 **可用性**？
* 用户在不在全球？
* 是 **内容分发** 还是 **计算资源**？
* 有没有出现 CloudFront / CDN / DNS？

---

## ✅ 一句话终极总结（考试 / 面试通用）

> AWS Regions 提供地理隔离，Availability Zones 提供高可用性，而 Edge Locations 通过内容分发网络将数据放在最靠近用户的位置以降低延迟。

---

如果你愿意，下一步我可以继续给你（同样 Markdown 可复制）：

* 🔥 **CloudFront + S3 + ALB 架构速记卡**
* 🧪 **10 道 Global Infrastructure 混合陷阱题**
* 🧠 **Region / AZ / Edge 终极决策树**

你直接选一句：
👉 **“要 PDF” / “再来 10 道陷阱题” / “做决策树”**
