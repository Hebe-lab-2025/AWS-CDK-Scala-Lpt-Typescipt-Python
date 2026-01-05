先上总结版，然后细讲 👇

---

## 一、YAML vs JSON 对比

### 1. 用途 & 风格

| 项目   | YAML                              | JSON                       |
| ---- | --------------------------------- | -------------------------- |
| 全称   | YAML Ain’t Markup Language        | JavaScript Object Notation |
| 主要用途 | **配置文件**（SpringBoot、K8s、Docker 等） | 数据传输、API 响应、前后端交互          |
| 风格   | 更像人写的配置，**缩进表示层级**                | 机器友好，**严格语法**，大括号中括号为主     |

---

### 2. 语法对比（直观示例）

**YAML：**

```yaml
server:
  port: 8080
  servlet:
    context-path: /api

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo
    username: root
    password: 123456
```

**JSON：**

```json
{
  "server": {
    "port": 8080,
    "servlet": {
      "context-path": "/api"
    }
  },
  "spring": {
    "datasource": {
      "url": "jdbc:mysql://localhost:3306/demo",
      "username": "root",
      "password": "123456"
    }
  }
}
```

**差异点：**

* YAML：

  * **不用大括号 / 中括号** 表示层级，用 **缩进 + 冒号**
  * 支持 **注释**：`# 这是注释`
* JSON：

  * 必须用 `{}` / `[]`
  * **不支持注释**（标准 JSON 里没有 `#` / `//`）
  * 每个字段之间要逗号，最后一个字段**不能多逗号**

---

### 3. 优缺点对比

**YAML 优点：**

* 配置更简洁、可读性好（特别是嵌套层级多时）
* 支持注释，适合写配置说明
* 在 SpringBoot / Docker / K8s 生态中是**默认、主流选择**

**YAML 缺点：**

* 对缩进敏感，容易因为 tab / 空格错位导致错误
* 语法更“宽松”，有时不小心写错不容易一眼看出
* 解析器比 JSON 略复杂

**JSON 优点：**

* 结构简单、统一，很适合机器处理
* 几乎所有语言都有原生 / 标准库支持
* 更适合 **API 数据**、日志、前后端交互

**JSON 缺点：**

* 不支持注释，不适合复杂配置说明
* 配置文件会比较“啰嗦”，嵌套多时很难读

---

## 二、YAML 在 Spring Boot / Docker / K8s 里的作用

### 1. Spring Boot 里：配置中心

**作用：**

* 用 `application.yml` 代替 `application.properties`
* 统一管理：**端口、数据源、Redis、日志、Profiles 等所有配置**

**示例：**

```yaml
server:
  port: 8081

spring:
  profiles:
    active: dev

  datasource:
    url: jdbc:mysql://localhost:3306/sky
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
```

**特点：**

* 层级结构清晰，看名字就知道是哪个模块：

  * `server.*` → 服务器相关
  * `spring.datasource.*` → 数据库
* 配合 `@ConfigurationProperties` 直接把 YAML 映射到 Java Bean：

```java
@ConfigurationProperties(prefix = "spring.datasource")
public class DataSourceProperties {
    private String url;
    private String username;
    private String password;
    // getter / setter
}
```

> 在 Java 后端面试里，常会问：**为什么用 yml 代替 properties？**

---

### 2. Docker 里：docker-compose.yml 描述多容器

**作用：**

* 用 YAML 描述多个容器服务：应用、数据库、缓存、消息队列等
* 一份文件定义好整个环境，**一条命令启动**：`docker-compose up -d`

**示例：**

```yaml
version: "3.8"

services:
  app:
    image: my-springboot-app:latest
    ports:
      - "8080:8080"
    depends_on:
      - mysql

  mysql:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: sky
    ports:
      - "3306:3306"
```

**关键点：**

* `services`：定义多个服务
* `depends_on`：声明依赖顺序
* `environment`：注入环境变量
* YAML 在这里就是 **“声明式环境描述语言”**

---

### 3. Kubernetes 里：资源清单（Manifest）

**作用：**

* 用 `.yaml` 文件定义所有 Kubernetes 资源，比如：

  * Deployment
  * Service
  * Ingress
  * ConfigMap
  * Secret
* 然后通过：`kubectl apply -f xxx.yaml` 部署

**Deployment 示例：**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sky-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: sky-app
  template:
    metadata:
      labels:
        app: sky-app
    spec:
      containers:
        - name: sky-app
          image: my-springboot-app:latest
          ports:
            - containerPort: 8080
```

**你可以这样总结：**

> * 在 **Spring Boot**：YAML 是应用的 **配置语言**
> * 在 **Docker Compose**：YAML 是环境编排的 **描述语言**
> * 在 **Kubernetes**：YAML 是集群资源的 **声明语言（desired state）**

---

## 三、YAML 相关面试题（DevOps / Java 后端）

下面这些你可以直接记成“题目 + bullet 要点回答”。

---

### 问题 1：YAML 和 JSON 有什么区别？什么时候用 YAML，什么时候用 JSON？

**要点：**

* 语法：YAML 用缩进表示层级，JSON 用 `{}`、`[]`
* 注释：YAML 支持 `#` 注释，JSON 不支持
* 用途：

  * 配置文件 → 选 YAML（Spring Boot、Docker、K8s）
  * API 数据 / 前后端交互 → 常用 JSON
* 易读性：YAML 更适合人读，JSON 更适合机器处理

---

### 问题 2：为什么 Spring Boot 推荐用 `application.yml` 而不是 `application.properties`？

**要点：**

* 层级清晰：`server.port`、`spring.datasource.url` 等更直观
* 嵌套配置多时更简洁，例如多个数据源、多个 Redis、复杂对象
* 更易扩展，配合 `@ConfigurationProperties` 映射为 Java 类

---

### 问题 3：YAML 中常见的坑有哪些？

**要点：**

* 使用 tab 缩进 → 应该只用 **空格**（通常 2 或 4 个空格）
* 缩进错层级 → 导致字段跑到别的对象里
* 冒号后面忘记空格：`key:value` vs `key: value`
* 字符串中有 `:`、`{}`、`[]` 时没有加引号

---

### 问题 4：说一下 docker-compose.yml 的作用和常见字段

**要点：**

* 作用：管理多个容器服务，定义依赖关系、一键启动
* 常见字段：

  * `services`：服务集合
  * `image` / `build`：镜像
  * `ports`：端口映射
  * `environment`：环境变量
  * `volumes`：挂载
  * `depends_on`：服务依赖

---

### 问题 5：Kubernetes 为什么大量使用 YAML？你用过哪些资源的 YAML？

**要点：**

* K8s 使用 **声明式 API**，YAML 很适合做“期望状态”的声明
* 资源例子：

  * `Deployment`：应用副本数、滚动更新策略
  * `Service`：暴露端口、负载均衡
  * `Ingress`：域名路由
  * `ConfigMap` / `Secret`：外部配置
* 运维 / DevOps 会把这些 YAML 文件放到 Git 做版本管理（GitOps）

---

### 问题 6（Amazon / DevOps 风格）：

**“你在项目中如何管理不同环境（dev / test / prod）的配置？YAML 在里面起到了什么作用？”**

**回答要点示例：**

* 使用 Spring Profiles：`application-dev.yml`、`application-prod.yml`
* 公共配置放在 `application.yml`，差异配置放在不同 profile 文件
* 配合环境变量 / K8s ConfigMap / Secret 覆盖敏感或环境相关配置
* 优点：

  * 避免把密码写死在代码中
  * 相同代码在不同环境通过 YAML 配置来“切换行为”

---

如果你愿意，下一步我可以帮你：

* 写一份 **完整的 Spring Boot `application.yml` 示例**（含 Druid、Redis、日志）
* 写一份 **Docker Compose + Spring Boot + MySQL** 的组合 YAML
* 或者做一套“面试官–候选人”对话版，帮你练习这些 YAML 相关问题 👀
