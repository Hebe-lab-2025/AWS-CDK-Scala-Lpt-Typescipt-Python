```
{
   "course":"AWS Lambda",
   "instructor":"Stephane Maarek",
   "instructor-full":{
      "first-name":"Stephane",
      "last-name":"Maarek",
      "awesomeness-level":9000
   },
   "sections-list":[
      "Introduction",
      "Deploying your first function",
      "Learning AWS Lambda in depth",
      "Real world examples"
   ],
   "lectures":[
      {
         "lecture-id":1,
         "lecture-name":"intro",
         "lecture-length":"5:03"
      },
      {
         "lecture-id":2,
         "lecture-name":"aws lambda",
         "lecture-length":"10:47"
      }
   ]
}
```
下面给你一组**从简单 → 实战 → CloudFormation 风格**的 **JSON ➜ YAML 对照例子**。
每个例子我都会点出**最容易错的地方**。

---

## 例子 1：最基础（对象）

### JSON

```json
{
  "name": "ec2",
  "count": 1
}
```

### YAML

```yaml
name: ec2
count: 1
```

✅ 规则

* `{}` ➜ 用缩进
* 数字不用引号

---

## 例子 2：嵌套对象（90% 人第一关）

### JSON

```json
{
  "server": {
    "host": "localhost",
    "port": 8080
  }
}
```

### YAML

```yaml
server:
  host: localhost
  port: 8080
```

⚠️ 易错点

* `host` / `port` **一定要多缩进一层**
* YAML 靠 **空格层级，不靠括号**

---

## 例子 3：数组（`[]` ➜ `-`）

### JSON

```json
{
  "ports": [22, 80, 443]
}
```

### YAML

```yaml
ports:
  - 22
  - 80
  - 443
```

📌 口诀

> **看到 `[`，脑子自动换成 `-`**

---

## 例子 4：数组 + 对象混合（很像 AWS）

### JSON

```json
{
  "securityGroups": [
    {
      "port": 22,
      "cidr": "0.0.0.0/0"
    },
    {
      "port": 80,
      "cidr": "0.0.0.0/0"
    }
  ]
}
```

### YAML

```yaml
securityGroups:
  - port: 22
    cidr: 0.0.0.0/0
  - port: 80
    cidr: 0.0.0.0/0
```

⚠️ 超级高频错

* `-` 后面的对象 **还是要缩进**
* 每个 `-` 代表一个 JSON object

---

## 例子 5：布尔值（AWS / K8s 很爱考）

### JSON

```json
{
  "enabled": true,
  "public": false
}
```

### YAML

```yaml
enabled: true
public: false
```

❗ 注意

* `true / false` **不要加引号**
* `"true"` 和 `true` 在 YAML 里是 **完全不同类型**

---

## 例子 6：字符串必须加引号的情况

### JSON

```json
{
  "date": "2026-01-10",
  "path": "/usr/local/bin",
  "text": "hello: world"
}
```

### YAML（安全写法）

```yaml
date: "2026-01-10"
path: "/usr/local/bin"
text: "hello: world"
```

⚠️ 为什么？

* `2026-01-10` 可能被当成 **date**
* `:` 在 YAML 里有语法意义

---

## 例子 7：CloudFormation 风格（你正在用的）

### JSON（CFN）

```json
{
  "Resources": {
    "MyInstance": {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "InstanceType": "t3.micro"
      }
    }
  }
}
```

### YAML（CFN）

```yaml
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t3.micro
```

💡 关键映射

| JSON    | YAML      |
| ------- | --------- |
| `{}`    | 缩进        |
| `:`     | key:      |
| `"xxx"` | xxx（通常可省） |

---

## 例子 8：JSON 引用 ➜ YAML 内建函数（进阶）

### JSON

```json
{
  "Ref": "MyInstance"
}
```

### YAML（等价写法）

```yaml
Ref: MyInstance
```

或（**推荐，CFN 常用**）👇

```yaml
!Ref MyInstance
```

---

## 终极速记表（你背这个）

```
{}        ➜ 缩进
[]        ➜ -
key:value ➜ key: value
数组元素  ➜ - item
结构靠空格，不靠符号
```

---

如果你愿意，下一步我可以直接：

1️⃣ 给你 **5 道 JSON → YAML 真·考试练习题**
2️⃣ 用 **你现在这个 EC2 + SG 模板**做一次完整转换
3️⃣ 帮你总结 **VS Code 画红线最常见 6 种 YAML 错误**

你选一个，我直接带你做。


```yaml
course: "AWS Lambda"
instructor: "Stephane Maarek"

instructor-full:
  first-name: "Stephane"
  last-name: "Maarek"
  awesomeness-level: 9000

sections-list:
  - "Introduction"
  - "Deploying your first function"
  - "Learning AWS Lambda in depth"
  - "Real world examples"

lectures:
  - lecture-id: 1
    lecture-name: "intro"
    lecture-length: "5:03"
  - lecture-id: 2
    lecture-name: "aws lambda"
    lecture-length: "10:47"
```

**你这份转换里最关键的点：**

* `instructor-full` 是嵌套对象 ➜ YAML 用缩进展开
* `sections-list` 是字符串数组 ➜ 用 `-`
* `lectures` 是“对象数组” ➜ 每个 lecture 用一个 `-`，里面的字段再缩进一层
* 像 `5:03` 这种带 `:` 的值我保留了引号，避免被 YAML 误解析


## 1️⃣ 5 道 JSON → YAML 真·考试练习题（带答案）

### 题 1：基础对象 + 数字/布尔

**JSON**

```json
{"name":"demo","enabled":true,"retries":3}
```

**YAML**

```yaml
name: "demo"
enabled: true
retries: 3
```

---

### 题 2：数组 + 对象数组

**JSON**

```json
{
  "ports":[80,443],
  "rules":[
    {"proto":"tcp","from":22,"to":22},
    {"proto":"tcp","from":80,"to":80}
  ]
}
```

**YAML**

```yaml
ports:
  - 80
  - 443

rules:
  - proto: "tcp"
    from: 22
    to: 22
  - proto: "tcp"
    from: 80
    to: 80
```

---

### 题 3：嵌套对象 + key 带连字符

**JSON**

```json
{
  "user-profile":{
    "first-name":"Meimei",
    "last-name":"Zhang"
  }
}
```

**YAML**

```yaml
user-profile:
  first-name: "Meimei"
  last-name: "Zhang"
```

---

### 题 4：值里有冒号/特殊字符（要加引号）

**JSON**

```json
{"time":"5:03","url":"http://example.com","note":"a:b:c"}
```

**YAML**

```yaml
time: "5:03"
url: "http://example.com"
note: "a:b:c"
```

---

### 题 5：空值/null + 空数组

**JSON**

```json
{"description":null,"items":[],"meta":{"owner":null}}
```

**YAML**

```yaml
description: null
items: []
meta:
  owner: null
```

---

## 2️⃣ 用你现在的 EC2 + SG CloudFormation 模板做一次“完整转换”（YAML ↔ JSON）

你现在用的是 CloudFormation **YAML**。下面给你一个**等价的 JSON 版本**（重点：`Ref`, `Fn::Base64`, `Fn::Sub` 这些在 JSON 里要写成对象）。

> ✅ 这份 JSON 对应你当前模板结构：Parameters + 2 个 SG + 1 个 EC2 + Outputs
> （含 UserData 的 Base64 + Sub）

```json
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Parameters": {
    "VpcId": { "Type": "AWS::EC2::VPC::Id" },
    "SubnetId": { "Type": "AWS::EC2::Subnet::Id" },
    "MyIpCidr": {
      "Type": "String",
      "Description": "Your public IP in CIDR, e.g. 1.2.3.4/32"
    }
  },
  "Resources": {
    "SSHSecurityGroup": {
      "Type": "AWS::EC2::SecurityGroup",
      "Properties": {
        "GroupDescription": "Enable SSH access from my IP only",
        "VpcId": { "Ref": "VpcId" },
        "SecurityGroupIngress": [
          {
            "IpProtocol": "tcp",
            "FromPort": 22,
            "ToPort": 22,
            "CidrIp": { "Ref": "MyIpCidr" }
          }
        ]
      }
    },
    "ServerSecurityGroup": {
      "Type": "AWS::EC2::SecurityGroup",
      "Properties": {
        "GroupDescription": "Allow HTTP from anywhere",
        "VpcId": { "Ref": "VpcId" },
        "SecurityGroupIngress": [
          {
            "IpProtocol": "tcp",
            "FromPort": 80,
            "ToPort": 80,
            "CidrIp": "0.0.0.0/0"
          }
        ]
      }
    },
    "MyInstance": {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "SubnetId": { "Ref": "SubnetId" },
        "ImageId": "ami-0742b4e673072066f",
        "InstanceType": "t3.micro",
        "SecurityGroupIds": [
          { "Ref": "SSHSecurityGroup" },
          { "Ref": "ServerSecurityGroup" }
        ],
        "UserData": {
          "Fn::Base64": {
            "Fn::Sub": "#!/bin/bash\nset -eux\nyum update -y\nyum install -y nginx\nsystemctl enable nginx\necho \"hello from $(hostname -f)\" > /usr/share/nginx/html/index.html\nsystemctl start nginx\n"
          }
        }
      }
    }
  },
  "Outputs": {
    "InstanceId": {
      "Value": { "Ref": "MyInstance" }
    }
  }
}
```

---

## 3️⃣ VS Code 画红线：最常见 6 种 YAML 错误（你一眼秒排）

1. **缩进不一致（最常见）**

* YAML 只能靠缩进表达层级
* 同一层级必须对齐（建议统一 2 spaces）

2. **Tab 当缩进**

* YAML 禁止 tab，VS Code 一红就多半是 tab
* 解决：设置 `"editor.insertSpaces": true`

3. **冒号后没空格**

* 错：`key:value`
* 对：`key: value`

4. **列表 `-` 缩进错位**

```yaml
SecurityGroupIngress:
- IpProtocol: tcp   # ❌ 常见（少缩进）
```

正确：

```yaml
SecurityGroupIngress:
  - IpProtocol: tcp
```

5. **字符串里有 `:` / `#` / `{}` 但没加引号**

* `5:03`、`http://`、`a:b`、`# comment`
* 保险做法：**加引号**：`"5:03"`

6. **CloudFormation 特有：`!Ref` / `!Sub` 写错位置**

* 比如把 `!Ref` 写到数组外/缩进不对
* 或者 `Fn::Base64: !Sub |` 的层级写错

---

如果你愿意，我下一步直接给你：

* **10 道更像 SAA-C03 的 JSON↔YAML 混合题（含陷阱）**
* 以及一个**“YAML 红线排错 30 秒流程”**（看到红线就按步骤查）
