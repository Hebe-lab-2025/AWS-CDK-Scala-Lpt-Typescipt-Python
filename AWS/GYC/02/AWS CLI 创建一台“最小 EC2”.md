## 1) 用 AWS CLI 创建一台“最小 EC2”（逐行解释，可直接照抄）

> 假设：你已经 `aws configure` 好（或在 CloudShell / 训练环境里已有权限）。
> 目标：**一台 t3.micro + 一个最小安全组 + 一对 key + 公网 IP（方便你 ssh）**。

### Step A：确认你现在在哪个 Region

```bash
aws configure get region
```

* 这就是默认 Region（例如 `us-west-2`）。
* 你后面创建的所有资源 **默认都在这个 Region**。

---

### Step B：拿到“默认 VPC”与一个子网（Subnet）

```bash
aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query "Vpcs[0].VpcId" --output text
```

* 返回一个 `vpc-xxxx`
* **VPC 是 Region 级别的网络**（跨多个 AZ）。

拿一个默认子网（一般每个 AZ 一个）：

```bash
aws ec2 describe-subnets --filters Name=default-for-az,Values=true --query "Subnets[0].SubnetId" --output text
```

* 返回 `subnet-xxxx`
* **Subnet 属于某一个 AZ**（单 AZ）。

---

### Step C：创建一个最小安全组（只放行 SSH 22 给你自己）

先创建 SG：

```bash
aws ec2 create-security-group \
  --group-name mini-ssh-sg \
  --description "minimal ssh sg" \
  --vpc-id <VPC_ID>
```

* `--vpc-id`：安全组必须挂在某个 VPC 上

放行 SSH（把 `<YOUR_IP>/32` 换成你公网 IP，比如 `1.2.3.4/32`）：

```bash
aws ec2 authorize-security-group-ingress \
  --group-id <SG_ID> \
  --protocol tcp --port 22 \
  --cidr <YOUR_IP>/32
```

* 这样比 `0.0.0.0/0` 安全很多（面试也爱听这个点）。

---

### Step D：创建 Key Pair（用来 SSH）

```bash
aws ec2 create-key-pair \
  --key-name mini-key \
  --query "KeyMaterial" --output text > mini-key.pem

chmod 400 mini-key.pem
```

* `mini-key.pem` 是私钥文件
* `chmod 400` 是 SSH 要求（权限太开放会被拒绝）。

---

### Step E：获取一个 AMI（推荐用 SSM 参数拿“最新 Amazon Linux 2023”）

```bash
aws ssm get-parameter \
  --name /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64 \
  --query "Parameter.Value" --output text
```

* 输出类似 `ami-0abc...`
* 好处：不用你手动查最新 AMI。

---

### Step F：启动最小 EC2

```bash
aws ec2 run-instances \
  --image-id <AMI_ID> \
  --instance-type t3.micro \
  --key-name mini-key \
  --security-group-ids <SG_ID> \
  --subnet-id <SUBNET_ID> \
  --associate-public-ip-address \
  --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=mini-ec2}]"
```

逐项解释：

* `--image-id`：系统镜像（AMI）
* `--instance-type`：机器规格（最小常用）
* `--key-name`：SSH 登录凭证
* `--security-group-ids`：入站/出站防火墙规则
* `--subnet-id`：放在哪个 AZ 的哪个子网
* `--associate-public-ip-address`：给公网 IP（方便 ssh）
* `--tag-specifications`：打 Name 标签（控制台里好找）

---

### Step G：查公网 IP，然后 SSH 登录

查实例 ID（只看你刚打的 Name）：

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=mini-ec2" "Name=instance-state-name,Values=pending,running" \
  --query "Reservations[0].Instances[0].InstanceId" --output text
```

查公网 IP：

```bash
aws ec2 describe-instances \
  --instance-ids <INSTANCE_ID> \
  --query "Reservations[0].Instances[0].PublicIpAddress" --output text
```

SSH：

```bash
ssh -i mini-key.pem ec2-user@<PUBLIC_IP>
```

---

### Step H：用完就删（省钱 & 干净）

终止实例：

```bash
aws ec2 terminate-instances --instance-ids <INSTANCE_ID>
```

（可选）删安全组/密钥：

```bash
aws ec2 delete-security-group --group-id <SG_ID>
aws ec2 delete-key-pair --key-name mini-key
rm -f mini-key.pem
```

---

## 2) Region / AZ / VPC 关系图（ASCII）

```text
AWS Global
  |
  +-- Region (e.g., us-west-2)  ✅ 地理大区
        |
        +-- AZ-a   ✅ 一个独立机房群
        |     |
        |     +-- Subnet-1 (public)
        |     +-- Subnet-2 (private)
        |
        +-- AZ-b
        |     |
        |     +-- Subnet-3 (public)
        |     +-- Subnet-4 (private)
        |
        +-- VPC  ✅ 逻辑网络边界（覆盖整个 Region）
              |
              +-- Route Tables / IGW / NAT / SG / NACL ...
```

关键点（考试/面试高频）：

* **Region**：资源“在哪里”（跨大区通常是跨地域）
* **AZ**：同一 Region 内的“故障隔离单元”
* **VPC**：Region 级别的私有网络；**Subnet 才是 AZ 级别**

---

## 3) CLI、SDK、控制台 三种方式统一成一个模型

把它当成同一个动作的 3 种“入口”，背后都是同一个 API：

```text
Console (点按钮)  \
CLI (aws ec2 ...)   --->  AWS API (RunInstances / CreateSG / etc.)  --->  Resource
SDK (Java/Python) /
```

对应关系（你可以这样理解）：

* **控制台**：帮你把参数填好，然后调用 API
* **CLI**：你自己在命令行把参数写出来，调用同一个 API
* **SDK**：你在代码里构造 request 对象，调用同一个 API

一句话总结：

> **Console/CLI/SDK 只是“参数怎么提供”的区别，最终都在调用同一个 AWS API。**

如果你把你现在的 Region（比如 `us-west-2`）和你想用的网络（默认 VPC 还是自己建的 VPC）告诉我，我可以把上面的命令替换成“你可以直接粘贴就能跑”的版本（把 `<VPC_ID>` 这些都自动查出来并串起来）。
