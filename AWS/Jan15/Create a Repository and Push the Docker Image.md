理解 AWS 计算服务 —— 从零到精通

下面是**完整中文翻译（偏实验/学习场景，忠于原意，适合记笔记或交作业）**：

---

### 理解 AWS 计算服务 —— 从零到精通

### 创建仓库并推送 Docker 镜像

**Amazon Elastic Container Service（ECS）** 是 AWS 提供的一项**完全托管的容器编排服务**。
它简化了容器化应用的部署和管理，使我们能够在大规模环境中运行 Docker 容器。
ECS 可以与其他 AWS 服务无缝集成，为部署和管理容器化工作负载提供**灵活且高效的解决方案**。

---

### ECS 在 AWS 生态中的定位

ECS 在 AWS 生态系统中有着独特的作用，**同时补充了 EC2 和 AWS Lambda**。

* **EC2** 提供的是虚拟服务器，需要用户手动进行配置和管理；
* **ECS** 提供容器编排能力，使容器化应用能够高效地部署和扩展；
* **AWS Lambda** 擅长事件驱动、短生命周期的任务，但并不适合长时间运行或有状态的应用。

因此，**ECS 提供了一个折中的方案**：
在抽象掉大量底层基础设施管理的同时，仍然为用户提供了足够的灵活性和执行环境控制能力，适用于非常广泛的应用场景。

---

### ECS 与 ECR 的集成

ECS 可以使用 **Amazon Elastic Container Registry（ECR）** 来管理容器镜像。
这种集成确保了容器在 ECS 上的**可靠、高效部署**，同时支持**镜像版本控制**，保证应用交付的一致性。

**Amazon Elastic Container Registry（ECR）** 是 AWS 提供的**托管式 Docker 容器镜像仓库服务**，
它提供安全、可扩展的仓库，用于存储、管理和部署 Docker 镜像。

---

## 创建仓库（Create a repository）

在本任务中，我们将创建一个 **ECR 私有仓库**。
完成后，基础设施结构应与下图所示一致。

### 创建私有仓库步骤：

1. 在 AWS 控制台搜索栏中搜索 **“ECR”**，选择 **Elastic Container Registry**
2. 在左侧导航栏中，选择 **Private registry → Repositories**
3. 点击 **Create repository**
4. 将仓库名称设置为 **computelab-ecs-repo**
5. 其余选项保持默认，点击 **Create**
6. **复制并保存仓库的 URI**，后续步骤会用到

---

## 推送 Docker 镜像（Push the Docker image）

接下来，我们将使用之前创建的 EC2 实例 **computelab-ec2-instance** 来构建 Docker 镜像。

### 连接 EC2 实例：

1. 在 AWS 控制台搜索 **EC2**，进入 EC2 Dashboard
2. 左侧选择 **Instances**，选中 **computelab-ec2-instance**
3. 点击 **Connect**
4. 连接方式选择 **EC2 Instance Connect**
5. 其余保持默认，点击 **Connect**

---

### 在 EC2 上推送 Docker 镜像到 ECR

#### 1️⃣ 安装 Docker

在 EC2 终端中运行提供的命令安装 Docker。

#### 2️⃣ 配置 AWS CLI

配置 AWS CLI 以便将镜像推送到 ECR 私有仓库。
运行命令并填写以下信息：

* 凭证：通过实验页面顶部计时器旁的钥匙图标获取
* Region：`us-east-1`
* Output format：`none`

#### 3️⃣ 设置账号 ID

切换到项目文件目录，并导出 AWS 账号 ID：

> 注意：将命令中的 `<ACCOUNT ID>` 替换为你自己的 AWS 账号 ID
> 可在 AWS 控制台右上角账号菜单中复制

#### 4️⃣ 创建 Docker 客户端

运行提供的命令创建 Docker 客户端。

#### 5️⃣ 构建 Docker 镜像

运行 Docker build 命令（该步骤约需 2–3 分钟）。

#### 6️⃣ 给镜像打 tag

为镜像打上指向 ECR 仓库的 tag。

#### 7️⃣ 推送镜像

运行 push 命令，将镜像推送到 ECR 仓库。

---

### 验证结果

1. 打开 **Elastic Container Registry** 控制台
2. 左侧选择 **Repositories**
3. 找到 **computelab-ecs-repo**
4. 保存 **Repository URI**，后续在 **Create a Task Definition** 中会用到

---

🎉 **恭喜！**
你已成功将 Docker 镜像推送到 ECR 私有仓库。
接下来，我们将使用该镜像在 **Amazon ECS** 中创建容器并部署应用。

---

如果你愿意，下一步我可以帮你做👇

* 🧠 **一句话总结 ECS 在 EC2 / Lambda 中的“中间层角色”（面试版）**
* 🧪 **把这套流程浓缩成 6 行 ECS + ECR 速记卡**
* 🔄 **直接接：Create Task Definition 每个字段为什么这么配（真考）**



Understanding AWS Compute Services — From Zero to Hero
Create a Repository and Push the Docker Image

Amazon Elastic Container Service (ECS) is a fully managed container orchestration service by AWS. 
It simplifies the deployment and management of containerized applications, 
allowing us to run Docker containers at scale. 

ECS integrates seamlessly with other AWS services, 
providing a flexible and efficient solution for deploying and managing containerized workloads.



ECS serves a unique purpose in the AWS ecosystem, 
complementing both EC2 instances and AWS Lambda. 

EC2 provides virtual servers that need manual provisioning and management, 
whereas ECS offers container orchestration, 
allowing for efficient deployment and scaling of containerized applications. 

On the other hand, AWS Lambda excels at event-driven, 
short-lived tasks but may not be suitable for long-running or stateful applications. 
ECS provides a middle ground, offering flexibility and control over the execution environment 
while still abstracting away much of the infrastructure management. 

Therefore, it’s valuable for a wide range of applications.

ECS can use an Amazon Elastic Container Registry (ECR) for container management. 

This integration ensures reliable and efficient deployment of containers on ECS, 
facilitating version control and ensuring consistency in application delivery.

Amazon Elastic Container Registry (ECR) is an AWS managed Docker container registry service. 
ECR provides a secure and scalable repository to store, manage, and deploy Docker images.



Create a repository
In this task, we’ll create a private repository in Amazon ECR. 
After completing this task, the provisioned infrastructure should look like the one shown in the figure below:


Provisioned infrastructure after creating an ECR repository

Provisioned infrastructure after creating an ECR repository

To create a private repository, follow the steps below:

- Look for “ECR” in the search bar and choose “Elastic Container Registry” from the search results.

- Go to “Repositories” under “Private registry” from the left sidebar.

- Click the “Create repository” button and set the repository name to computelab-ecs-repo.

- Leave all other settings as default, and click the “Create” button at the bottom of the page.

- Copy and save the “URI” of the repository. We’ll use it later.

Push the Docker image

Now, we’ll use an EC2 instance created earlier, computelab-ec2-instance, to build our Docker image.

- Go to the AWS Management Console and search for the “EC2” option.
- Click the “EC2” service from the search results to access the EC2 dashboard.

- Click the “Instances” option on the left sidebar under the “Instances” section. 
- Select computelab-ec2-instance and click the “Connect” button.


Make sure the connection type is “Connect using EC2 Instance Connect.”

Leave everything else at its default setting and select “Connect” at the end.

Follow the steps below to push the Docker image to computelab-ecs-repo:

In the EC2 instance terminal, run the commands given below to install Docker:

Ace Editor
Next, proceed with configuring the AWS CLI on the EC2 instance, 
which will enable us to push our image to the ECR private repository. 

Run the command given below and provide the required credentials. 
To access the credentials, click the key icon next to the lab timer on the top of this page. 
Provide us-east-1 as the region and none for the output format.

Ace Editor
Now, run the command given below to switch to the files and export your account ID required to build the Docker image:

Ace Editor
Note: Replace <ACCOUNT ID> with your account ID in the command above. 
Click the top-right corner in the AWS Management Console. Copy <ACCOUNT ID> from the drop-down menu.

Execute the command below to create a Docker client:

Ace Editor
Next, run the command given below to build a Docker image.

Note: This step takes 2–3 minutes to complete.

Ace Editor
After the build is completed, tag the image to push it to the repository:

Ace Editor
Lastly, run the following command to push this image to the repository:

Ace Editor
Navigate to the “Elastic Container Registry” console on AWS and click the “Repositories” option 
in the left-hand navigation pane.

Look for computelab-ecs-repo and save the “Repository URI” for later. 
We’ll need it in the “Create a Task Definition” task.

Congratulations on successfully pushing the Docker image to the ECR private repository. 
We’ll now use this image to create a container in Amazon ECS and deploy our application.


Test
Next
Educative: AI-Powered Interactive Courses for Developers
