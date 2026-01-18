### `sudo apt update`

**翻译：**

> 使用管理员权限，更新系统的软件包列表。

---

### 这条命令在做什么？

* **`sudo`**：以管理员（root）权限执行
* **`apt`**：Ubuntu 的包管理器
* **`update`**：
  👉 **刷新软件源索引**（不安装、不升级）

也就是说：

> 让系统“知道”**现在有哪些新版本的软件可以装**。

---

### 为什么要先执行它？

在安装任何软件前（如 `mysql-client`、`psql`、`docker`）：

* 避免找不到包
* 避免安装到旧版本

---

### 常见对比（别混）

| 命令                | 作用      |
| ----------------- | ------- |
| `apt update`      | 更新软件列表  |
| `apt upgrade`     | 升级已安装软件 |
| `apt install xxx` | 安装软件    |

---

### 一句话记忆

> **apt update = 更新“清单”，不是更新“软件”。**


### `sudo apt install mysql-client-core-8.0`

**翻译：**

> 使用管理员权限，安装 **MySQL 8.0 的客户端核心工具**。

---

### 这条命令在做什么？

* **`sudo`**：以管理员权限执行
* **`apt install`**：安装软件包
* **`mysql-client-core-8.0`**：

  * MySQL **客户端**（不是服务器）
  * 只包含 **连接和操作数据库所需的最小工具**
  * 不会在本机启动 MySQL 服务

👉 **常用于从 EC2 连接 RDS MySQL**

---

### 为什么装的是 *client*，不是 *server*？

* RDS 已经是 **托管的 MySQL Server**
* 你只需要一个工具去 **连它**
* 安装 server 会：

  * 多占资源
  * 还可能端口冲突（3306）

---

### 安装完成后你可以做什么？

```bash
mysql --version
```

```bash
mysql -h <rds-endpoint> -u <username> -p
```

---

### 一句话面试版

> **mysql-client-core installs only the MySQL client tools,
> which is ideal for connecting to RDS without running a local MySQL server.**

如果你愿意，下一步我可以直接帮你检查：
👉 **从 EC2 连 RDS MySQL 必须满足的 3 个条件（90% 卡在这）**


