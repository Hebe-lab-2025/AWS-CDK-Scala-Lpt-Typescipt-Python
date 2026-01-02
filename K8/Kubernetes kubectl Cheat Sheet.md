太好了 👍
我给你两个内容，一次性可直接放到 **GitHub README**：

1️⃣ **Kubernetes kubectl Cheat Sheet（中英双语）**
2️⃣ **Docker 面试题版 README（题目+参考答案要点版）**

——全部已经是标准 Markdown，可直接复制。

---

## 🧭 Kubernetes kubectl Cheat Sheet

### 🗂️ Cluster & Context

| Command                             | Description                    | 中文解释           |
| ----------------------------------- | ------------------------------ | -------------- |
| `kubectl version`                   | Show client and server version | 显示客户端和服务端版本    |
| `kubectl config get-contexts`       | List contexts                  | 查看当前所有 context |
| `kubectl config use-context <name>` | Switch context                 | 切换当前集群         |
| `kubectl cluster-info`              | Display cluster info           | 显示集群信息         |
| `kubectl get nodes`                 | List cluster nodes             | 查看所有节点         |
| `kubectl describe node <name>`      | Node details                   | 查看节点详细信息       |

---

### 📦 Pod 操作

| Command                          | Description    | 中文解释         |
| -------------------------------- | -------------- | ------------ |
| `kubectl get pods`               | List all Pods  | 查看所有 Pod     |
| `kubectl get pods -A`            | All namespaces | 查看所有命名空间 Pod |
| `kubectl describe pod <name>`    | Details of Pod | 查看 Pod 详情    |
| `kubectl logs <pod>`             | View logs      | 查看日志         |
| `kubectl logs -f <pod>`          | Stream logs    | 持续滚动日志       |
| `kubectl exec -it <pod> -- bash` | Enter Pod      | 进入容器交互       |

---

### 🧩 Deployment / ReplicaSet

| Command                                      | Description       | 中文解释          |
| -------------------------------------------- | ----------------- | ------------- |
| `kubectl get deploy`                         | List deployments  | 查看 Deployment |
| `kubectl create deploy <name> --image=<img>` | Create deployment | 创建 Deployment |
| `kubectl scale deploy <name> --replicas=3`   | Scale replicas    | 扩容/缩容         |
| `kubectl rollout status deploy <name>`       | Check rollout     | 查看滚动发布状态      |
| `kubectl rollout undo deploy <name>`         | Rollback          | 回滚版本          |

---

### 🌐 Service & Networking

| Command                                                      | Description    | 中文解释       |
| ------------------------------------------------------------ | -------------- | ---------- |
| `kubectl get svc`                                            | List Services  | 查看 Service |
| `kubectl expose deploy <name> --port=80 --type=LoadBalancer` | Expose service | 暴露服务       |
| `kubectl port-forward <pod> 8080:80`                         | Local port map | 端口转发       |

---

### 🗂️ Namespace

| Command                    | Description      | 中文解释   |
| -------------------------- | ---------------- | ------ |
| `kubectl get ns`           | List namespaces  | 查看命名空间 |
| `kubectl create ns <name>` | Create namespace | 创建命名空间 |
| `kubectl delete ns <name>` | Delete namespace | 删除命名空间 |

---

### 📝 YAML 常用

| Command                         | Description     | 中文解释    |
| ------------------------------- | --------------- | ------- |
| `kubectl apply -f <file>.yaml`  | Apply config    | 创建/更新资源 |
| `kubectl delete -f <file>.yaml` | Delete resource | 删除资源    |
| `kubectl get pod -o yaml`       | Output details  | 输出 YAML |
| `kubectl explain pod`           | API docs        | 查看字段含义  |

---

## 🐳 Docker Interview Q&A – README 版

> 适合 GitHub 项目 / 面经整理 / 课堂资料

---

### ❓ 1. What is Docker and why is it used?

**Answer key points**

* containerization platform
* lightweight vs VM
* portability
* isolation
* Dev → Prod consistency

---

### ❓ 2. Difference between Docker image and container?

| Image              | Container        |
| ------------------ | ---------------- |
| blueprint          | running instance |
| immutable          | mutable          |
| stored in registry | lives in runtime |

---

### ❓ 3. What is Dockerfile?

Key points:

* text file describing image build
* base image
* instruction list
* reproducible builds

---

### ❓ 4. Common Dockerfile instructions?

* `FROM`
* `RUN`
* `COPY`
* `WORKDIR`
* `CMD`
* `ENTRYPOINT`
* `EXPOSE`

---

### ❓ 5. CMD vs ENTRYPOINT?

| CMD                 | ENTRYPOINT                |
| ------------------- | ------------------------- |
| default args        | main command              |
| can be overridden   | harder to override        |
| good for “defaults” | good for “fixed behavior” |

---

### ❓ 6. What is Docker Compose?

Answer:

* define multi-container apps
* YAML based
* `docker-compose up`
* networking built-in

---

### ❓ 7. How do you persist data in Docker?

* **volumes** (recommended)
* bind mounts
* tmpfs

---

### ❓ 8. What is the difference between volume and bind mount?

| Volume            | Bind mount            |
| ----------------- | --------------------- |
| managed by Docker | host path             |
| portable          | environment-dependent |
| safer             | powerful but risky    |

---

### ❓ 9. How do you reduce Docker image size?

* Alpine base image
* multistage builds
* remove cache
* `.dockerignore`

---

### ❓ 10. What is Docker registry?

* remote image storage
* Docker Hub / ECR / GCR / ACR

---

### ❓ 11. What is container orchestration?

* manage many containers
* scheduling
* scaling
* failure recovery

👉 examples: **Kubernetes, ECS, Nomad**

---

### ❓ 12. How does Docker networking work?

Key points:

* bridge network (default)
* host network
* overlay network (Swarm/K8s)
* port mapping

---

### ❓ 13. How to debug a failing container?

* `docker logs`
* `docker ps -a`
* `docker inspect`
* `docker exec -it bash`
* check healthcheck

---

### ❓ 14. What are health checks?

* `HEALTHCHECK` in Dockerfile
* status monitored
* restart policies

---

### ❓ 15. How do you secure Docker containers?

* run as non-root
* scan images
* minimal base image
* read-only filesystem
* signatures

---

## ✅ 你还可以要：

* 📌 Kubernetes 面试题 50+
* 🧭 DevOps 全链路图
* 🧪 真题模拟卷
* 🧊 Notion 模板版
* 🎯 高级 kubectl + helm 速查表
