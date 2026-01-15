下面这份是**面试即用 + 生产可落地**版。

---

## 🧪 3 道 tag / image ID 易混面试题（高频）

### 题 1

> 两个镜像 tag 不同，但 image ID 一样，说明什么？

**✅ 正确结论：**

* **同一个镜像内容**，只是**不同“名字（tag）”指向它**
* `docker tag` **不会复制镜像**

**面试金句：**

> *Tags are references; image ID represents the actual content.*

---

### 题 2

> 我 push 了 `latest`，但 ECS 还在跑旧代码，为什么？

**最可能原因（按概率）：**

1. **ECS 不会自动拉新镜像**（Service 未强制新部署）
2. Task Definition **未创建新 revision**
3. 容器实例/缓存导致未重新 pull（较少）

**正确做法：**

* 新建 Task Definition revision
* 或 `Force new deployment`

---

### 题 3

> 删除了 `latest` tag，会删除镜像吗？

**❌ 不一定**

* **只删 tag 引用**
* 只要还有其他 tag 指向该 image ID → 镜像仍存在

**面试补刀：**

> *Image lifecycle is independent from tags.*

---

## 🔁 `latest` vs `versioned tag`（生产对比，必背）

| 维度      | `latest`    | Versioned Tag（v1.2.3 / sha） |
| ------- | ----------- | --------------------------- |
| 是否确定性   | ❌ 不确定       | ✅ 确定                        |
| 回滚能力    | ❌ 困难        | ✅ 秒级                        |
| 审计 / 排错 | ❌ 不可追踪      | ✅ 可追溯                       |
| 并发部署    | ❌ 容易踩坑      | ✅ 安全                        |
| 生产推荐    | ❌ 不推荐       | ✅ 强烈推荐                      |
| 适合场景    | 本地开发 / Demo | **生产环境**                    |

**一句话结论：**

> **`latest` 是指针，不是版本。**

---

## 🧠 CI/CD 里 tag 应该怎么定（Git SHA / SemVer）

### ✅ 推荐的**生产级组合策略（最稳）**

> **一个 image，多重 tag：**

```text
my-app:1.4.2
my-app:1.4
my-app:1
my-app:git-9f3a21c
```

* **SemVer**：人类友好（release / 回滚）
* **Git SHA**：机器友好（唯一、可审计）

---

### 🔹 Git SHA（强烈推荐）

**优点：**

* 唯一、不可变
* 完美对应源码

**示例：**

```bash
docker build -t my-app:git-${GIT_SHA} .
docker push my-app:git-${GIT_SHA}
```

**面试话术：**

> *Git SHA guarantees immutability and traceability.*

---

### 🔹 SemVer（产品/发布友好）

**优点：**

* 易沟通、易回滚
* 与发布节奏一致

**注意：**

* **只能在 release 时打**
* 不能随意覆盖

**示例：**

```bash
docker tag my-app:git-${SHA} my-app:1.4.2
```

---

### ❌ 不推荐：只用 `latest`

**原因：**

* 不可追溯
* 易被覆盖
* 回滚成本高

---

## 🧠 面试官最爱追问 & 标准答案

**Q：你在生产用什么 tag 策略？**

> *We tag images with both Git SHA for immutability and SemVer for release management, and avoid relying on `latest` in production.*

**Q：那 `latest` 还有用吗？**

> *Only for local development or quick demos, never as a deployment target.*

---

## 🔚 30 秒总结（背这一段）

> *Tags are just pointers to image IDs. In production, I avoid `latest` and instead use immutable tags like Git SHA for traceability, optionally combined with SemVer for releases. This makes deployments deterministic and rollbacks safe.*

---

如果你愿意，我可以下一步直接给你：

* 🧪 **5 道“ECS 没更新到底是谁的锅”排错题**
* 🧠 **一张 Docker build → tag → push → deploy 的白板流程图**
* 🔁 **ECR lifecycle policy + tag 策略最佳实践**

你选一个，我马上展开。
