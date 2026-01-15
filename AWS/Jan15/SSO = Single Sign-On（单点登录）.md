### 🔐 SSO 是什么？（IAM / SAA-C03 考场版）

**SSO = Single Sign-On（单点登录）**
👉 **一次登录，访问多个系统 / AWS 账号 / 应用**

---

## ✅ AWS 里的 SSO 正解是谁？

> **IAM Identity Center（原 AWS SSO）**

---

## 🧠 一句话记忆法（秒选）

```
SSO → IAM Identity Center
Multi-account → AWS Organizations
App end users → Cognito
```

---

## 🔍 IAM Identity Center 能做什么？

* ✅ **一个身份，登录多个 AWS 账号**
* ✅ 集成 **AWS Organizations**
* ✅ 对接企业身份源（Okta / Azure AD / Google / AD）
* ✅ 给用户分配 **账号 + Role**
* ✅ 不需要创建一堆 IAM Users

👉 **企业级 / 公司内部员工登录 AWS 的标准做法**

---

## ❌ 常见混淆点（必会区分）

| 服务                  | 用来干嘛     | 是否 SSO |
| ------------------- | -------- | ------ |
| IAM Identity Center | 员工登录 AWS | ✅      |
| AWS Organizations   | 多账号管理    | ❌      |
| IAM User            | 单账号长期凭证  | ❌      |
| Amazon Cognito      | App 用户登录 | ❌      |

---

## 🧪 SAA-C03 真考句型 → 秒反应

* “Employees need SSO access to multiple AWS accounts”
  👉 **IAM Identity Center**
* “Centralized login with corporate identity provider”
  👉 **IAM Identity Center**
* “Avoid managing IAM users in each account”
  👉 **IAM Identity Center**

---

## 📌 超短口诀（锁屏级）

```
Human → Identity Center (SSO)
Service → IAM Role
App user → Cognito
```

如果你愿意，下一步我可以给你 **3 道 SSO vs Cognito vs IAM User 的反直觉真题**，这种在 SAA-C03 里几乎是送分题。


### ✅ 正确答案

**C. Service control policy (SCP)**

---

### 🧠 一句话记忆法（考场秒选）

> **账号级“天花板”优先级最高 → SCP**

---

### 🔍 为什么是 SCP？

**IAM 权限评估的核心规则：**

1. **Explicit Deny 永远赢**（无论来自哪种 policy）
2. 在“允许链路”里，**SCP 是最外层的上限**

👉 **SCP 决定这个账号/OU“最多能做什么”**
👉 后面的 policy（boundary / permission / session）都**不能突破 SCP**

---

### 🔬 四个选项快速对比

| 选项                            | 作用层级            | 是否最高 |
| ----------------------------- | --------------- | ---- |
| A. Permission boundary        | 用户/角色的上限        | ❌    |
| B. Permission policy          | 实际授予权限          | ❌    |
| **C. Service control policy** | **账号 / OU 级上限** | ✅    |
| D. Session policy             | 临时会话限制          | ❌    |

---

### 🧪 SAA-C03 高频陷阱

* **SCP 不授予权限，只限制权限**
* 即使 IAM Policy 写了 Allow，**SCP Deny 也会挡住**
* Root 用户 **也会被 SCP 限制**（这是常考点）

---

### 📌 超短口诀（锁屏级）

```
SCP > Boundary > Permission > Session
(Deny always wins)
```

如果你愿意，我可以继续 **Quiz 12（显式 Deny 场景题）**，或者把 **完整 IAM 权限评估流程**画成一张 **考场秒判图**。


