下面用**中文 + 架构直觉 + 考试友好**的方式，把 **Fault Isolation Boundary（故障隔离边界）** 一次讲清楚，并帮你**一眼分清 Zonal / Regional / Global**。

---

## 一、什么是 Fault Isolation Boundary？

**一句话理解：**
👉 **Fault Isolation Boundary = “故障最多会影响到哪里”**

* 定义了**故障被限制（隔离）在什么范围内**
* AWS 通过不同层级的隔离，避免**一个点出问题拖垮全部系统**

---

## 二、三种故障隔离边界（从小到大）

![Image](https://docs.aws.amazon.com/images/whitepapers/latest/aws-fault-isolation-boundaries/images/a-zonal-service-with-zonally-isolated-control-planes-and-data-planes.png)

![Image](https://blog.sysfore.com/wp-content/uploads/2015/12/AWS-Global-Infrastructure-Table.png)

![Image](https://docs.aws.amazon.com/images/whitepapers/latest/aws-fault-isolation-boundaries/images/availability-zones.png)

### 1️⃣ Zonal Services（可用区级）

**定义：**

* 资源**只存在于一个 Availability Zone（AZ）**
* **一个 AZ 的故障，不会影响其他 AZ**
* 但：**该 AZ 内的资源会一起受影响**

**典型服务：**

* **Amazon EC2**
* **Amazon EBS**

**直觉理解：**
👉 放在**一栋楼里**的服务器
👉 这栋楼出事，楼里的都受影响

📌 **设计原则：**

> Zonal 服务 **必须跨 AZ 部署** 才高可用

---

### 2️⃣ Regional Services（区域级）

**定义：**

* 资源**天然跨多个 AZ**
* 对外提供**一个 Region 级访问入口**
* **单个 AZ 故障，服务仍然可用**

**典型服务：**

* **Amazon DynamoDB**
* **Amazon S3**
* **Amazon RDS（Multi-AZ）**

**直觉理解：**
👉 一个**城市级系统**
👉 一片城区断电，城市还能运转

📌 **设计优势：**

> Regional 服务 **自带高可用（HA）**

---

### 3️⃣ Global Services（全球级）

**定义：**

* **不属于任何单一 Region**
* 全球统一存在
* **某个 Region 挂了，服务仍然存在**

**典型服务：**

* **AWS Identity and Access Management（IAM）**
* **Amazon Route 53**
* **Amazon CloudFront**

**直觉理解：**
👉 **全球级控制系统**
👉 一个城市没了，不影响全球系统

---

## 三、一张表秒懂（考试必背）

| 隔离边界         | 故障影响范围    | 示例服务           | 是否默认 HA |
| ------------ | --------- | -------------- | ------- |
| **Zonal**    | 单个 AZ     | EC2 / EBS      | ❌       |
| **Regional** | 单个 Region | S3 / DynamoDB  | ✅       |
| **Global**   | 不限 Region | IAM / Route 53 | ✅       |

---

## 四、用“挂了会怎样”来反推（非常好用）

* **一个 AZ 挂了就没了？** → Zonal
* **一个 AZ 挂了还能用？** → Regional
* **一个 Region 挂了还能用？** → Global

---

## 五、把原文翻成“人话”

> AWS 把服务按“故障最多影响到哪里”分成三类：
> **只影响一个 AZ 的叫 Zonal，
> 覆盖整个 Region 的叫 Regional，
> 不依赖 Region 的叫 Global。**
> 搞清楚这一点，才能设计真正**高可用、抗故障**的架构。

---

如果你愿意，我可以继续帮你：

* 🧠 做一张 **Fault Isolation Boundary 一页速记图（Zone / Region / Global）**
* 🧪 出 **5 道 Zonal vs Regional vs Global 高频陷阱题**
* 📄 整理成 **Markdown / PDF 复习卡**
