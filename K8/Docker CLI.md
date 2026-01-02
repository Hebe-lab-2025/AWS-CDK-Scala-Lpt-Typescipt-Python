很好 👍
我给你一份**完全可直接粘贴到 GitHub README.md 的版本**（只有 Markdown 内容，无多余说明）。

你只需要👇整体复制即可。

---

## 🐳 Docker CLI Cheat Sheet

### Docker & 常用命令

| Command                        | Description                       | 中文解释               |
| ------------------------------ | --------------------------------- | ------------------ |
| `curl localhost`               | Pings the application             | 访问本地运行的服务，测试是否能连通  |
| `docker build`                 | Builds an image from a Dockerfile | 根据 Dockerfile 构建镜像 |
| `docker build . -t <tag>`      | Builds and tags the image         | 构建镜像并打标签           |
| `docker container rm <id>`     | Removes a container               | 删除容器               |
| `docker images`                | Lists the images                  | 查看本地所有镜像           |
| `docker ps`                    | Lists running containers          | 查看正在运行的容器          |
| `docker ps -a`                 | Lists all containers              | 查看包括已退出的所有容器       |
| `docker pull <image>`          | Pulls latest image from registry  | 从镜像仓库拉取镜像          |
| `docker push <image>`          | Pushes image to registry          | 将镜像推送到远端仓库         |
| `docker run <image>`           | Runs a command in new container   | 启动一个新容器            |
| `docker run -p host:container` | Publishes ports                   | 将容器端口映射到主机         |
| `docker stop <id>`             | Stops running containers          | 停止容器               |
| `docker stop $(docker ps -q)`  | Stops all running containers      | 一次性停止全部容器          |
| `docker tag src dst`           | Creates new tag for image         | 给镜像新打一个标签          |
| `docker --version`             | Displays Docker version           | 显示 Docker 版本       |

### Shell / 环境命令

| Command                     | Description             | 中文解释      |
| --------------------------- | ----------------------- | --------- |
| `exit`                      | Closes terminal session | 退出终端 / 容器 |
| `export MY_NAMESPACE=value` | Export env variable     | 设置环境变量    |
| `ls`                        | List files              | 查看当前目录文件  |

### Git 命令

| Command            | Description      | 中文解释     |
| ------------------ | ---------------- | -------- |
| `git clone <repo>` | Clone repository | 克隆远程代码仓库 |

### IBM Cloud Container Registry

| Command                  | Description                | 中文解释                |
| ------------------------ | -------------------------- | ------------------- |
| `ibmcloud cr images`     | Lists registry images      | 查看容器镜像仓库中的镜像        |
| `ibmcloud cr login`      | Docker login to registry   | 登录 IBM Cloud 容器镜像仓库 |
| `ibmcloud cr namespaces` | View accessible namespaces | 查看你拥有的命名空间          |
| `ibmcloud cr region-set` | Set working region         | 设置目标区域              |
| `ibmcloud target`        | Show current account info  | 查看当前账户目标信息          |
| `ibmcloud version`       | Show CLI version           | 查看 IBM Cloud CLI 版本 |

---

如果你愿意，我还能：

* ⭐ 加上 **emoji 版本 README**
* 📌 自动生成 **目录 TOC**
* 🐳 再加 **Kubernetes kubectl Cheat Sheet**
* 🎓 出一套 Docker 面试题版 README
