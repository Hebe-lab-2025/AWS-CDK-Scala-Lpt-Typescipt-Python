太好了 👍
下面我直接给你一套**「一页就够」的 AWS 速记材料**，**可直接复制到 Markdown / 打印成 PDF**，专门用来 **考试秒选**。

---

# 📄 AWS Compute & Service Model 一页速记（Exam Ready）

## 一、IaaS / PaaS / SaaS / FaaS 极速对照表

| 模型       | 你负责什么          | AWS 负责什么    | 典型服务               | 一句话记忆     |
| -------- | -------------- | ----------- | ------------------ | --------- |
| **IaaS** | OS、Runtime、App | 服务器、网络、存储   | EC2                | **你管系统**  |
| **PaaS** | 代码             | OS、运行环境、扩缩容 | Elastic Beanstalk  | **你只写代码** |
| **SaaS** | 使用             | 一切          | Gmail / Salesforce | **直接用**   |
| **FaaS** | 函数代码           | 服务器、执行、扩缩容  | Lambda             | **事件触发**  |

---

## 二、🧪 AWS 考试陷阱题（IaaS vs PaaS 高频混淆）

### ❌ 陷阱 1：看到“部署应用”就选 EC2

**题干关键词**：

> deploy web app quickly, no server management

✅ **正确反应**：
👉 **PaaS（Elastic Beanstalk）**

❌ 错因：

* EC2 需要你装 OS、配置环境（太重）

---

### ❌ 陷阱 2：看到“需要控制 OS”却选 PaaS

**题干关键词**：

> custom OS, kernel-level access

✅ **正确反应**：
👉 **IaaS（EC2）**

❌ 错因：

* PaaS 不允许你碰 OS

---

### ❌ 陷阱 3：看到“自动扩缩容”就误选 Lambda

**题干关键词**：

> long-running application, stateful

✅ **正确反应**：
👉 **EC2 + Auto Scaling / PaaS**

❌ 错因：

* Lambda 有执行时间限制、无状态

---

### ❌ 陷阱 4：看到“最少运维”却还选 EC2

**题干关键词**：

> minimal operational overhead

✅ **正确反应**：
👉 **PaaS 或 FaaS**

---

## 三、🧠「关键词 → 秒选模型」反射表（核心）

### 🔑 看到这些词，**直接条件反射**

| 关键词                              | 秒选            |
| -------------------------------- | ------------- |
| **manage OS / install packages** | IaaS (EC2)    |
| **no server management**         | PaaS          |
| **just upload code**             | PaaS          |
| **event-driven**                 | FaaS (Lambda) |
| **pay per execution**            | FaaS          |
| **full control**                 | IaaS          |
| **fastest to deploy**            | PaaS          |
| **stateless**                    | FaaS          |
| **long-running process**         | EC2           |
| **background job / trigger**     | Lambda        |

---

## 四、考试“反直觉”总规则（必背）

> ❗ **AWS 考试不是问“你能不能用 EC2 做”，
> 而是问“AWS 有没有更托管、更省事的方案”。**

👉 **能不用 EC2，就不要选 EC2。**

---

## 五、30 秒自检（考前脑内过一遍）

* 我是不是被 **EC2 诱导** 了？
* 题目有没有说 **no server management**？
* 有没有 **event / trigger / pay per execution**？
* 有没有明确说 **need OS control**？

---

### ✅ 一句话终极口诀

> **要 OS → IaaS
> 只写代码 → PaaS
> 事件触发 → FaaS
> 直接用 → SaaS**

---

如果你愿意，下一步我可以帮你：

* 🔥 把这页 **导出成 PDF**
* 🧪 再给你 **10 道“只看关键词秒选”的真考风格题**
* 🧠 做一张 **Compute + Storage + DB 联合决策树**

你直接选一句：
👉 **“要 PDF” / “来 10 道陷阱题” / “做联合决策树”**
