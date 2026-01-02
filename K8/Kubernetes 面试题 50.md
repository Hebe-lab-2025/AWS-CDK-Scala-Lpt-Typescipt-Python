太好了 👍 我一次性给你三样内容（可直接放 README / Notion）：

* 📌 **Kubernetes 面试题 50+（含要点答案）**
* 🧭 **DevOps 全链路图（ASCII + 说明）**
* 🧪 **Kubernetes 真题模拟卷（含答案）**

---

# 📌 Kubernetes 面试题 50+（要点版）

> 每题都有简短答案要点，便于记忆与展开

---

### 1. What is Kubernetes?

* container orchestration platform
* scheduling, scaling, healing, networking

---

### 2. Difference between Docker Swarm and Kubernetes?

* K8s: complex, more features, de-facto standard
* Swarm: simpler, fewer features

---

### 3. What is a Pod?

* smallest deployable unit
* 1+ containers tightly coupled
* share network + volume

---

### 4. Pod vs Container?

* pod contains one or more containers
* pod is scheduling unit
* containers share namespace inside pod

---

### 5. What is a Node?

* machine where pods run
* worker node has kubelet & kube-proxy

---

### 6. What is a Cluster?

* control plane + worker nodes

---

### 7. What is Deployment?

* manages replica sets
* declarative updates of pods
* rolling updates

---

### 8. What is StatefulSet?

* ordered pods
* stable identity & storage
* databases / Kafka / Zookeeper

---

### 9. Deployment vs StatefulSet?

| Deployment         | StatefulSet     |
| ------------------ | --------------- |
| stateless          | stateful        |
| no stable identity | stable hostname |
| random scheduling  | ordered         |

---

### 10. What is DaemonSet?

* runs one pod per node
* used for logging / monitoring / agents

---

### 11. ReplicaSet vs Deployment?

* Deployment manages ReplicaSet
* Do not use ReplicaSet directly usually

---

### 12. What is Service in Kubernetes?

* stable virtual IP for pods
* load balancing
* service discovery

---

### 13. Types of Kubernetes Services?

* ClusterIP (default)
* NodePort
* LoadBalancer
* ExternalName

---

### 14. What is Ingress?

* HTTP/HTTPS routing layer
* works at L7
* supports TLS & host rules

---

### 15. Difference: Ingress vs Service?

* Service: exposes pods
* Ingress: exposes services to internet

---

### 16. What is kubelet?

* runs on every node
* ensures containers are running

---

### 17. What is etcd?

* key-value store
* backs Kubernetes control plane
* stores cluster state

---

### 18. What is Control Plane?

* API server
* etcd
* scheduler
* controller manager

---

### 19. What is kubectl?

* CLI to talk to API server

---

### 20. Explain Namespace

* logical isolation
* resource quotas
* multi-tenant cluster separation

---

### 21. What are ConfigMaps?

* store configuration (non-secret)

---

### 22. What are Secrets?

* base64 encoded sensitive data
* use KMS for real encryption

---

### 23. ConfigMap vs Secret?

* secret for passwords/tokens
* configmap for normal config

---

### 24. What is Horizontal Pod Autoscaler?

* scales pod count based on CPU/memory/custom metrics

---

### 25. What is Vertical Autoscaler?

* adjusts pod resource limits

---

### 26. What is Cluster Autoscaler?

* scales worker nodes
* cloud-integration required

---

### 27. How does Kubernetes achieve self-healing?

* restarts failed containers
* reschedules pods
* replaces unhealthy nodes

---

### 28. Readiness vs Liveness Probe?

| Liveness       | Readiness                  |
| -------------- | -------------------------- |
| restart pod    | remove from service        |
| check if alive | check if ready for traffic |

---

### 29. What happens when liveness probe fails?

* container restarts

---

### 30. What happens when readiness probe fails?

* removed from load balancing

---

### 31. What is Taint and Toleration?

* taint node = “do not schedule”
* toleration allows scheduling anyway
* used for special nodes

---

### 32. What is Affinity/Anti-Affinity?

* schedule pods closer or separate
* topology-aware policies

---

### 33. What is Persistent Volume (PV)?

* cluster storage resource

---

### 34. What is Persistent Volume Claim (PVC)?

* user request for storage

---

### 35. PV vs PVC?

| PV            | PVC           |
| ------------- | ------------- |
| admin-managed | user-requests |
| resource      | claim         |

---

### 36. What is StorageClass?

* dynamic provisioning of PV

---

### 37. Difference between Rolling Update and Recreate?

* rolling = zero downtime
* recreate = downtime occurs

---

### 38. Canary Deployment?

* release to small percentage first

---

### 39. Blue-Green Deployment?

* two environments live
* instant switch traffic

---

### 40. What is Sidecar Pattern?

* helper container in same pod
* logging / proxy / envoy / istio sidecar

---

### 41. What is Service Mesh?

* Istio / Linkerd
* traffic shaping
* mTLS
* observability

---

### 42. What is CSI?

* container storage interface
* plugin model for storage providers

---

### 43. Troubleshooting command for pod?

* `kubectl describe pod`
* `kubectl logs`
* `kubectl exec`

---

### 44. CrashLoopBackOff causes?

* bad image
* failing entrypoint
* liveness wrong
* missing permissions

---

### 45. ImagePullBackOff causes?

* wrong image name
* registry auth error
* no internet access

---

### 46. How do you limit resources?

* requests / limits in pod spec

---

### 47. What if requests > node capacity?

* pod remains Pending

---

### 48. What is Helm?

* package manager for Kubernetes
* like apt/yum for k8s

---

### 49. What is Operator?

* Kubernetes application controller
* automates life-cycle (DBs etc.)

---

### 50. What is API Server?

* central point of communication
* all kubectl commands go here

---

### 51. What is OPA / Gatekeeper?

* policy enforcement
* compliance / security rules

---

# 🧭 DevOps 全链路图（ASCII）

```
Developer
    │
    ▼
Code  ───► Git ───► Pull Request ───► Code Review
                                      │
                                      ▼
                               CI Pipeline (Build & Test)
                                      │
                        ┌─────────────┴─────────────┐
                        ▼                           ▼
                 Unit Tests                   Security Scan
                        │                           │
                        └─────────────┬─────────────┘
                                      ▼
                                Build Artifact
                                      │
                                      ▼
                              Container Image
                                      │
                                      ▼
                              Push to Registry
                                      │
                                      ▼
                             CD Pipeline (Deploy)
                                      │
                        ┌─────────────┴──────────────┐
                        ▼                            ▼
                 Staging Env                    Production Env
                        │                            │
                        ▼                            ▼
                Monitoring & Logs             Alerts & Dashboards
                        │
                        ▼
                 Feedback to Dev Team
```

**核心环节：**

* Git & PR
* CI（Build + Test + Scan）
* Image Registry
* CD（自动化部署）
* K8s / Cloud runtime
* Monitoring / Logging / Tracing
* Feedback loop

---

