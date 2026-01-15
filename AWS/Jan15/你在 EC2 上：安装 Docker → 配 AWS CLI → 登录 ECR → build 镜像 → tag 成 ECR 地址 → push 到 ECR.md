下面我把这段 Lab 里出现/隐含的 **所有 command** 按真实执行顺序拆开讲（每条：**干嘛用 + 为什么要做 + 常见坑**）。你照这个写笔记/面试复述都够。

> 你已经跑过一次了，我这里按“标准正确姿势”讲清楚。

---

## 0) 先说明你在做什么（一句话）

你在 **EC2 上**：安装 Docker → 配 AWS CLI → 登录 ECR → build 镜像 → tag 成 ECR 地址 → push 到 ECR。

---

## 1) 更新系统 + 安装 Docker + 启动 Docker

### ✅ Command（你跑的三连）

```bash
sudo yum -y update && \
sudo yum install -y docker && \
sudo service docker start
```

### 逐条解释

1. `sudo yum -y update`

* **做什么**：更新系统软件包（安全补丁、依赖库等）
* `-y`：自动回答 yes（避免中途停下来问你）
* **为啥要做**：减少依赖冲突、安装 Docker 更稳

2. `sudo yum install -y docker`

* **做什么**：安装 Docker Engine（以及它依赖的 containerd、runc 等）
* **为什么**：后面要 `docker build / tag / push`

3. `sudo service docker start`

* **做什么**：启动 Docker 服务（让 `dockerd` 跑起来）
* **注意**：在 Amazon Linux 2023 上，本质是转到 systemd（你输出里也看到 `Redirecting to /bin/systemctl ...`）
* **验证**（建议你记住这两条）：

  ```bash
  sudo systemctl status docker
  sudo docker version
  ```

### 常见坑

* docker 没启动 → 后面 `docker build` 会报 `Cannot connect to the Docker daemon`
* 权限问题：不想每次 sudo，可以把用户加进 docker 组（lab 可不做）：

  ```bash
  sudo usermod -aG docker ec2-user
  # 然后重新登录 session 才生效
  ```

---

## 2) 配置 AWS CLI（让 EC2 能调用 AWS API）

### ✅ Command

```bash
aws configure
```

### 你会被问 4 个值

* AWS Access Key ID
* AWS Secret Access Key
* Default region name：`us-east-1`
* Default output format：`none`（或 `json`，lab 要你填 none）

### 这条命令背后做了什么

把配置写进：

* `~/.aws/credentials`
* `~/.aws/config`

### 常见坑（非常重要）

* **不要把 key 贴到任何地方**（截图/聊天都不要）
* region 填错 → 后面 ECR 登录可能会失败（比如你 repo 在 us-east-1 但你填了 us-west-2）

---

## 3) 设置账号 ID（为了拼出 ECR 仓库域名）

### ✅ Command（lab 模板）

```bash
export account_id=<Account ID>
```

### 解释

* `export`：把变量放到当前 shell 环境里
* 之后你就能用 `$account_id` 代替那串数字
* ECR registry 域名格式固定：

  ```
  <account_id>.dkr.ecr.<region>.amazonaws.com
  ```

### 验证

```bash
echo $account_id
```

### 常见坑

* 变量没设置/拼错 → 后面的 login/tag/push 都会错

---

## 4) 登录 ECR（让 Docker 有权限 push 镜像）

### ✅ Command（关键命令）

```bash
aws ecr get-login-password --region us-east-1 | \
sudo docker login --username AWS --password-stdin \
$account_id.dkr.ecr.us-east-1.amazonaws.com
```

### 这条命令分两段理解

#### A) `aws ecr get-login-password --region us-east-1`

* **做什么**：向 AWS 要一个**临时的登录 token**（不是你的 access key）
* 输出是一段很长的密码字符串

#### B) `| sudo docker login ... --password-stdin ...`

* `|`：把左边输出“管道”给右边当输入
* `docker login`：把凭证写到 Docker 的配置里（所以后面 push 不会 403）
* `--password-stdin`：从标准输入读密码（安全，避免命令行历史记录泄露）
* `--username AWS`：ECR 固定写 AWS
* 目标 registry：`$account_id.dkr.ecr.us-east-1.amazonaws.com`

### 常见坑（面试也爱问）

* **region 写错** → token 对不上 registry
* **IAM 权限不足** → 会报 AccessDenied（需要 `ecr:GetAuthorizationToken`）
* 你用 `sudo docker login` 会把凭证写到 `/root/.docker/config.json`（lab 提示的 warning 就是这个）

---

## 5) Build 镜像（把代码变成 Docker Image）

### ✅ Command

```bash
sudo docker build -t computelab-ecs-repo .
```

### 解释

* `docker build`：按当前目录的 `Dockerfile` 构建镜像
* `-t computelab-ecs-repo`：给镜像起名字（tag），默认 `:latest`
* `.`：build context（把当前目录内容发给 docker 引擎用于 COPY 等）

### 常见坑

* Dockerfile 路径不对（不在当前目录）
* build context 太大（上传慢）
* node/npm 依赖慢（正常）

### 验证

```bash
sudo docker images
```

---

## 6) Tag 镜像（把本地镜像“贴上 ECR 地址”）

### ✅ Command

```bash
sudo docker tag computelab-ecs-repo:latest \
$account_id.dkr.ecr.us-east-1.amazonaws.com/computelab-ecs-repo:latest
```

### 解释

* 你 build 出来的镜像名叫：`computelab-ecs-repo:latest`
* ECR 要求推送的镜像名必须是这种格式：

  ```
  <registry>/<repository>:<tag>
  ```
* 所以 `docker tag` 是把同一个镜像“起个新名字”，并不复制内容

### 常见坑

* repo 名拼错（ECR 里叫 computelab-ecs-repo）
* account_id/region 写错

### 验证

```bash
sudo docker images | grep computelab-ecs-repo
```

---

## 7) Push 镜像到 ECR（真正上传）

### ✅ Command（lab 最后一步）

```bash
sudo docker push \
$account_id.dkr.ecr.us-east-1.amazonaws.com/computelab-ecs-repo:latest
```

### 解释

* Docker 会把镜像按 layer 分块上传
* 上传成功后，你在 ECR 控制台能看到 image digest / tag

### 常见坑

* 没 login 或 token 过期 → `no basic auth credentials`
* ECR 权限不足 → `denied: User ... is not authorized to perform ecr:InitiateLayerUpload`
  （需要 push 相关权限：`InitiateLayerUpload / UploadLayerPart / CompleteLayerUpload / PutImage`）

---

## 8) 你要保存的“Repository URI”是什么？

你在控制台看到的 **Repository URI** 一般是：

```
214228787083.dkr.ecr.us-east-1.amazonaws.com/computelab-ecs-repo
```

### 为什么要保存它？

下一步 “Create a Task Definition” 里要填 **Image URI**，一般写成：

```
<Repository URI>:latest
```

也就是：

```
.../computelab-ecs-repo:latest
```

---

## ✅ 最后：把整套命令浓缩成你能背的 4 行（面试版）

```bash
sudo yum install -y docker && sudo systemctl enable --now docker
aws configure
aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin $account_id.dkr.ecr.us-east-1.amazonaws.com
sudo docker build -t computelab-ecs-repo . && sudo docker tag computelab-ecs-repo:latest $account_id.dkr.ecr.us-east-1.amazonaws.com/computelab-ecs-repo:latest && sudo docker push $account_id.dkr.ecr.us-east-1.amazonaws.com/computelab-ecs-repo:latest
```

---

如果你想更“实战”，我可以继续把下一关 **Task Definition** 里每个字段（CPU/Memory/Port/Log driver/TaskRole/ExecutionRole）怎么填、为什么这么填，用“真考追问”的方式给你拆解。
