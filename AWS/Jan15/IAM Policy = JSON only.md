### ✅ 正确答案

**D. IAM Policy**

---

### 🧠 一句话记忆法（考场用）

> **IAM 实体（Entity）= 能被直接赋权的身份**
> **Policy = 权限规则，不是身份**

---

### 🔍 逐个选项快速拆解

| 选项                     | 是否 IAM Entity | 原因                           |
| ---------------------- | ------------- | ---------------------------- |
| **A. IAM Role**        | ✅ 是           | Role 是一种身份，可被服务/用户/账号 assume |
| **B. IAM Users Group** | ✅ 是           | Group 是用户集合，本身是 IAM 实体       |
| **C. IAM User**        | ✅ 是           | User 是最基础的 IAM 身份            |
| **D. IAM Policy**      | ❌ 否           | Policy 只是**权限文档**，不能被 assume |

---

### 🧪 真考陷阱提醒

* **Policy ≠ Entity**（这是 IAM 最经典陷阱之一）
* **Entity = User / Group / Role**
* **Policy 是 attach 给 entity 的**

---

### 📌 超短口诀（秒选）

```
User / Group / Role = Entity
Policy = Permission document
```

如果你要，我可以继续给你 **5 道 IAM 高频陷阱判断题（SAA-C03 真考风）**，专门练这种“看着都对但只能选一个”的题。


