这是**云 + DevOps 里最容易混、但面试很爱考**的一组词。
我先给你**一句话结论**，再拆 **异同 + 场景 + 面试用法**。

---

## 🧠 一句话结论

> **Launch 偏“启动基础资源”，Deploy 偏“发布应用代码”。**

---

## 1️⃣ Launch 是什么？

**核心含义**

* 把一个“模板”变成**正在运行的资源**
* 强调 **资源生命周期的开始**

**典型对象**

* EC2 instance
* ECS task
* Lambda execution（不常说，但本质也是）

**例子**

* Launch an EC2 instance
* Launch a task on ECS

👉 含义：

> **让资源跑起来**

---

## 2️⃣ Deploy 是什么？

**核心含义**

* 把 **应用 / 代码 / 配置** 放到运行环境中
* 强调 **软件交付**

**典型对象**

* Web app
* Container image
* Lambda function code

**例子**

* Deploy a React application
* Deploy a container to ECS
* Deploy a Lambda function

👉 含义：

> **把代码发布上线**

---

## 3️⃣ 最关键的区别（表格版）

| 维度       | Launch          | Deploy         |
| -------- | --------------- | -------------- |
| 关注点      | 基础设施            | 应用代码           |
| 偏向       | Infra / Compute | App / Software |
| 是否一定有代码  | ❌               | ✅              |
| 是否一定有服务器 | 通常有             | 不一定            |
| 常见搭配     | instance / task | app / service  |

---

## 4️⃣ 放到你正在做的 Lab 里（非常重要）

你这套 Lab 其实**两个词都用到了**👇

### ① Launch

* **Launch an EC2 instance**
* **Run / Launch an ECS task**

👉 启动算力资源

---

### ② Deploy

* Deploy React app
* Deploy Docker container to ECS

👉 把你的应用跑在这些资源上

---

## 5️⃣ 一个完整流程（你可以直接背）

```
1. Launch EC2 instance
2. Build Docker image
3. Push image to ECR
4. Deploy container to ECS
```

👉 **Launch ≠ Deploy，但常常先 Launch 再 Deploy**

---

## 6️⃣ 面试级标准答案（中英文）

### 中文

> Launch 通常指启动或创建计算资源，而 Deploy 指将应用或代码发布到运行环境中。

### 英文

> Launch usually refers to starting infrastructure resources, while deploy refers to releasing application code to a runtime environment.

---

## 7️⃣ 一个常见误区（帮你避坑）

❌ 说：

> “I deployed an EC2 instance”

✅ 更准确：

> “I launched an EC2 instance and deployed my application on it.”

---

如果你愿意，我可以下一步帮你：

* 🧪 出 5 道 **launch vs deploy 面试陷阱题**
* 🧠 用一句话区分 **run / start / launch / deploy**
* 🔄 把你 Lab 的每一步标注成 **Launch 还是 Deploy**

你想继续 **抠术语** 还是 **直接模拟面试问法**？
