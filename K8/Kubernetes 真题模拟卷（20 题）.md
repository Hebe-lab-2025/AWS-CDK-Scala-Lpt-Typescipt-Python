# 🧪 Kubernetes 真题模拟卷（20 题）

👉 单选 + 场景 + 考点覆盖全面
👉 末尾附标准答案

---

### Q1

A pod is repeatedly restarting with CrashLoopBackOff. What is the first command to inspect the issue?

A. kubectl get nodes
B. kubectl describe pod
C. kubectl get svc
D. kubectl top pod

---

### Q2

Traffic should NOT be routed to a pod until it finishes initialization. What should you configure?

A. Liveness probe
B. Readiness probe
C. Startup probe
D. PreStop hook

---

### Q3

You need per-pod unique persistent storage. Which resource is required?

A. Deployment
B. StatefulSet
C. ReplicaSet
D. DaemonSet

---

### Q4

Which ensures only one pod runs per node?

A. StatefulSet
B. DaemonSet
C. Job
D. CronJob

---

### Q5

Which Service type exposes externally through cloud provider LB?

A. ClusterIP
B. NodePort
C. LoadBalancer
D. Ingress

---

### Q6

Which object stores cluster state?

A. kubelet
B. etcd
C. API server
D. scheduler

---

### Q7

Best way to deploy DB in Kubernetes?

A. Deployment
B. StatefulSet
C. CronJob
D. Job

---

### Q8

Which ensures minimum downtime releases?

A. Recreate
B. RollingUpdate
C. Blue-Green only
D. Delete & recreate

---

### Q9

Command to see full YAML of pod?

A. kubectl view
B. kubectl config
C. kubectl get pod -o yaml
D. kubectl dump

---

### Q10

Which enables dynamic volume provisioning?

A. PVC
B. PV
C. StorageClass
D. Node selector

---

### Q11

Which component schedules pods?

A. controller manager
B. API server
C. scheduler
D. kube-proxy

---

### Q12

How do you exec into pod?

A. ssh pod
B. kubectl attach
C. kubectl exec -it pod -- bash
D. docker exec

---

### Q13

Which explains API resource documentation?

A. kubectl info
B. kubectl explain
C. kubectl docs
D. kubectl api

---

### Q14

Multi-tenant isolation recommendation?

A. Nodes
B. Pods
C. Namespaces
D. StorageClasses

---

### Q15

Job vs CronJob difference?

* Job runs once
* CronJob runs on schedule

---

### Q16

Pod pending reason?

A. Image not found
B. Node full
C. CrashLoop
D. All above

---

### Q17

Ingress works at which OSI layer?

A. L3
B. L4
C. L7
D. L1

---

### Q18

What is `kube-proxy` used for?

* service networking & routing

---

### Q19

Best practice for secrets?

* use Secret + KMS encryption

---

### Q20

Which command shows node resources?

A. kubectl resource
B. kubectl capacity
C. kubectl top nodes
D. kubectl show nodes

---

## ✅ 标准答案

1-B
2-B
3-B
4-B
5-C
6-B
7-B
8-B
9-C
10-C
11-C
12-C
13-B
14-C
15– explained
16-D
17-C
18 – service routing
19 – Secret + KMS
20-C

---

如果你愿意，我还能：

* 🔥 再出 **100 道高级场景题**
* 📚 整理 **K8s + DevOps 完整学习路线**
* 📌 给你做 **闪卡记忆版**
* 🎯 生成 **PDF 可打印版**
