太棒了 👍
下面给你一份 **可直接放 GitHub README / Notion 的高级速查表**：

✔️ kubectl 进阶用法
✔️ helm 常见操作 + 模板调试
✔️ 日常排障高频命令
✔️ 含中英解释

> 全部是 **Markdown 格式、可直接复制粘贴**

---

# 🎯 高级 kubectl + Helm 速查表

---

## 🧭 kubectl 高级用法 Cheat Sheet

### 🔹 一行查看所有资源（按命名空间）

| Command                            | Meaning                      | 中文解释        |
| ---------------------------------- | ---------------------------- | ----------- |
| `kubectl get all -n <ns>`          | list everything in namespace | 查看命名空间内所有资源 |
| `kubectl get all --all-namespaces` | list across cluster          | 查看整个集群所有资源  |

---

### 🔹 使用 label 过滤

| Command                            | Meaning         | 中文解释          |
| ---------------------------------- | --------------- | ------------- |
| `kubectl get pods -l app=myapp`    | filter by label | 按 label 查 Pod |
| `kubectl delete pod -l app=myapp`  | delete by label | 按 label 删除    |
| `kubectl label pod mypod env=prod` | add label       | 给 Pod 打 label |

---

### 🔹 JSONPath / 高级输出

| Command                                                   | Meaning            | 中文解释          |
| --------------------------------------------------------- | ------------------ | ------------- |
| `kubectl get pod -o wide`                                 | extra node/IP info | 显示 Pod 详细调度信息 |
| `kubectl get pod -o json`                                 | full JSON          | JSON 全部信息     |
| `kubectl get pod -o jsonpath='{.items[*].metadata.name}'` | JSONPath           | 返回 Pod 名字列表   |

---

### 🔹 快速排障组合技

| Command                                                    | Meaning            | 中文解释    |
| ---------------------------------------------------------- | ------------------ | ------- |
| `kubectl describe pod <name>`                              | event + details    | 查看事件与状态 |
| `kubectl logs <pod>`                                       | container logs     | 查看日志    |
| `kubectl logs -f <pod>`                                    | stream logs        | 持续追踪日志  |
| `kubectl exec -it <pod> -- bash`                           | enter container    | 进入容器    |
| `kubectl get events --sort-by=.metadata.creationTimestamp` | show latest errors | 查看最新事件  |

---

### 🔹 重启 Deployment

| Command                                 | Meaning         | 中文解释 |
| --------------------------------------- | --------------- | ---- |
| `kubectl rollout restart deploy <name>` | restart rollout | 平滑重启 |
| `kubectl rollout status deploy <name>`  | rollout status  | 查看状态 |
| `kubectl rollout undo deploy <name>`    | rollback        | 回滚版本 |

---

### 🔹 资源占用排查

| Command                        | Meaning         | 中文解释      |
| ------------------------------ | --------------- | --------- |
| `kubectl top nodes`            | node usage      | 查看节点资源    |
| `kubectl top pods`             | pod usage       | 查看 Pod 资源 |
| `kubectl top pod --containers` | container level | 查看容器资源    |

> 需要 metrics-server

---

### 🔹 强制删除卡住的 Pod

| Command                                              | 中文解释 |
| ---------------------------------------------------- | ---- |
| `kubectl delete pod <name> --force --grace-period=0` | 强制删除 |

---

### 🔹 快速创建资源（不写 YAML）

| Command                                                 | 中文解释           |
| ------------------------------------------------------- | -------------- |
| `kubectl create deploy nginx --image=nginx`             | 快速建 Deployment |
| `kubectl expose deploy nginx --port=80 --type=NodePort` | 直接暴露服务         |

---

### 🔹 Diff / Dry-run

| Command                                      | 中文解释   |
| -------------------------------------------- | ------ |
| `kubectl apply -f app.yaml --dry-run=client` | 只验证不执行 |
| `kubectl diff -f app.yaml`                   | 查看变更差异 |

---

## 🚀 Helm 高级速查表

---

### 🔹 helm 基础操作

| Command                           | Meaning       | 中文解释     |
| --------------------------------- | ------------- | -------- |
| `helm repo add myrepo <url>`      | add repo      | 添加仓库     |
| `helm repo update`                | update index  | 更新仓库     |
| `helm search repo nginx`          | search charts | 搜索 Chart |
| `helm install myapp myrepo/chart` | install chart | 安装       |
| `helm upgrade myapp myrepo/chart` | upgrade       | 更新       |
| `helm uninstall myapp`            | uninstall     | 卸载       |

---

### 🔹 查看发布信息

| Command                 | 中文解释   |
| ----------------------- | ------ |
| `helm list`             | 查看已安装  |
| `helm status myapp`     | 查看状态   |
| `helm history myapp`    | 查看历史版本 |
| `helm rollback myapp 1` | 回滚到版本  |

---

### 🔹 Helm + values.yaml

| Command                                          | 中文解释         |
| ------------------------------------------------ | ------------ |
| `helm show values mychart`                       | 显示默认 values  |
| `helm install myapp mychart -f values.yaml`      | 使用自定义 values |
| `helm upgrade myapp mychart -f values-prod.yaml` | prod 配置      |

---

### 🔹 本地调试模板（非常重要🔥）

| Command                 | 中文解释                |         |
| ----------------------- | ------------------- | ------- |
| `helm template mychart` | 渲染模板输出              |         |
| `helm template mychart  | kubectl apply -f -` | 本地渲染再部署 |
| `helm lint mychart`     | 检查 Chart 语法         |         |

---

### 🔹 Helm 与 Kubernetes 结合排错

| Command                   | 中文解释        |
| ------------------------- | ----------- |
| `helm get manifest myapp` | 查看生成的 YAML  |
| `helm get values myapp`   | 查看当前 values |
| `helm get hooks myapp`    | 查看 hooks    |

---

## 🧠 实战排障套路（送你）

### ❌ 步骤卡住 / CrashLoopBackOff

```
kubectl describe pod
kubectl logs <pod>
```

### 🚫 Pending

```
kubectl describe pod
kubectl get nodes
kubectl describe node
```

### 🐢 访问超时

```
kubectl get svc
kubectl describe svc
kubectl get ep
```

### 🧊 CPU / 内存打满

```
kubectl top pod
kubectl top node
```

---

## ✅ 你接下来可以要：

* 📌 **kubectl + helm 面试题 50+**
* 🧾 YAML 模板合集（Deployment/Ingress/StatefulSet）
* 🧠 K8s 真题 100 题
* 🧊 Notion 模板版速查表
* 🛠️ 实战场景练习题（含答案）

只要告诉我你要哪一个 😊
