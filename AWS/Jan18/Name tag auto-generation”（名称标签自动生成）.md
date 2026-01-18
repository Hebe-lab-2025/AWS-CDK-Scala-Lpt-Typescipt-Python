在 **“Name tag auto-generation”**（名称标签自动生成）部分，在输入框中填写 **RDS**，以将该 **VPC 命名为 `RDS-vpc`**。


**10.0.0.0/16 的解释（AWS / 网络高频）：**

### 含义

* **10.0.0.0**：网络地址
* **/16**：CIDR 前缀，表示前 **16 位是网络位**

### 实际范围

* **IP 范围**：`10.0.0.0` ～ `10.0.255.255`
* **可用 IP 数量**：

  * 总数：2^(32−16) = **65,536**
  * 在 VPC 中可用稍少（AWS 预留 5 个）

### 常见用途

* **私有网段（RFC 1918）**
* 常用于 **VPC CIDR block**
* 适合后续划分多个子网（/24、/20 等）

### 一句话速记

> **10.0.0.0/16 = 一个常见的 VPC 私有地址段，可拆成 256 个 /24 子网。**


**翻译：**

redirected to the "Create VPC workflow" page
被重定向到 **“Create VPC workflow”** 页面。

offloads the burden of database administration tasks.
**翻译：**

**将数据库管理任务的负担卸载出去 / 减轻数据库管理工作的负担。**

### 更自然的技术表达

* **把数据库运维工作交由云服务处理**
* **无需自行承担数据库管理任务**
* **降低数据库运维复杂度**

### 面试一句话版

> *RDS offloads the burden of database administration tasks such as backups, patching, and maintenance.*
> **RDS 将备份、补丁和维护等数据库运维工作交由 AWS 处理。**







