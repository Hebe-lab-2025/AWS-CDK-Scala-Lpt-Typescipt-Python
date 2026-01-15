## 🧪 3 道「为什么网页打不开」排错题（真考风）

### 题 1：EC2 上 Nginx 正常跑着，但浏览器访问 `http://<PublicIP>` 超时

**已知：**

* EC2 在 public subnet，有 Public IP
* Security Group inbound 已开 `0.0.0.0/0 -> TCP:80`
* 访问依然超时（timeout）

**问：最可能的 3 个根因是什么？你按什么顺序排查？（写出“要看哪里 + 期望看到什么”）**

> 参考排查路径（标准答案要点）

1. **Route Table**：public subnet 的路由表是否有 `0.0.0.0/0 -> IGW`（没有就必超时）
2. **NACL**：是否同时放行 **入站 80** + **出站 ephemeral ports（1024-65535）**（出站不放行会导致握手/返回包被挡）
3. **OS 防火墙 / 服务监听**：EC2 内部 `curl localhost:80` 是否通；`ss -lntp | grep :80` 是否监听在 `0.0.0.0:80`（只监听 127.0.0.1 外部也访问不到）

---

### 题 2：ALB 域名能打开，但一直 502；Target Group 显示 unhealthy

**已知：**

* ALB listener 80 -> Target Group (HTTP:80)
* EC2 上服务健康（本机 `curl localhost:80` 正常）
* Target Group unhealthy，ALB 返回 502

**问：最可能的 3 个根因是什么？**

> 要点

1. **EC2 SG 没允许来自 ALB 的流量**：EC2 的 inbound 80 应该允许 **ALB 的 Security Group**（而不是随便开/或没开）
2. **健康检查路径/端口不对**：health check path 配成 `/health` 但服务只提供 `/`；或 target 实际监听 8080
3. **实例实际在 private subnet / 路由问题**：ALB 子网/实例子网选择错误，或 NACL 阻断返回端口

---

### 题 3：网页偶尔能打开，偶尔超时；同一 VPC 两台实例表现不一致

**问：你如何判断是 NACL 问题还是 SG 问题？给出“证据型”判断方法。**

> 要点（考试喜欢）

* **SG 是 stateful**：只要入站允许，返回流量自动允许；不需要额外开出站 ephemeral
* **NACL 是 stateless**：入站允许不够，**出站也必须放行返回端口范围**
* **证据**：

  * 如果现象是“握手/返回包缺失、间歇性超时”，优先怀疑 **NACL 出站 ephemeral** 或过严规则
  * 对比两台实例所在 subnet 的 **NACL 关联** 是否不同，是最快定位方式

---

## 🔁 Security Group vs NACL（考试高频对比表）

| 维度      | Security Group (SG)       | Network ACL (NACL)           |
| ------- | ------------------------- | ---------------------------- |
| 作用对象    | **ENI/实例级**（网卡）           | **Subnet 级**                 |
| 状态      | **Stateful**（允许入站则返回自动允许） | **Stateless**（入站/出站都要显式允许）   |
| 规则顺序    | **无顺序**（只要有一条允许就行）        | **有顺序**（从小到大匹配，先匹配先生效）       |
| 默认行为    | 默认拒绝入站；出站通常允许（看你怎么配）      | 有默认 NACL；自定义通常默认拒绝更严格        |
| 支持 Deny | ❌ 不能显式 deny（只有 allow）     | ✅ 支持 allow/deny              |
| 最常见坑    | 只开了入站，忘了来源应是 **ALB SG**   | 只开了入站 80，忘了出站 **1024-65535** |
| 用途定位    | “谁能访问这台机器/这个 ENI”         | “这个 subnet 的粗粒度网络边界”         |

---

## 🧠 把“开网页访问”这一步改写成 CloudFormation / Terraform 版本

下面给你一个**最小可用**版本：

* 一个 VPC + 公网子网 + IGW + 路由表
* 一个 SG：允许 HTTP(80) 从公网进
* 一个 EC2：UserData 安装并启动 Nginx

> 你可以把它当成“网页打不开”排错时的标准对照基线。

---

### ✅ CloudFormation（YAML）最小可用示例

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Minimal public web server (EC2 + Nginx) with VPC/Subnet/IGW/Route

Parameters:
  LatestAmiId:
    Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
    Default: /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64

Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsHostnames: true
      EnableDnsSupport: true

  InternetGateway:
    Type: AWS::EC2::InternetGateway

  AttachIgw:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref VPC
      InternetGatewayId: !Ref InternetGateway

  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref VPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true
      AvailabilityZone: !Select [0, !GetAZs ""]

  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref VPC

  PublicDefaultRoute:
    Type: AWS::EC2::Route
    DependsOn: AttachIgw
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway

  PublicSubnetRouteAssoc:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet
      RouteTableId: !Ref PublicRouteTable

  WebSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTP from Internet
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
      # 出站默认允许；若你公司策略要求锁死再按需放行

  WebInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !Ref LatestAmiId
      InstanceType: t3.micro
      SubnetId: !Ref PublicSubnet
      SecurityGroupIds:
        - !Ref WebSecurityGroup
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          dnf -y update
          dnf -y install nginx
          systemctl enable nginx
          systemctl start nginx
          echo "hello from CFN" > /usr/share/nginx/html/index.html

Outputs:
  WebUrl:
    Description: Open this in browser
    Value: !Sub "http://${WebInstance.PublicIp}"
```

---

### ✅ Terraform（HCL）最小可用示例

```hcl
terraform {
  required_providers {
    aws = { source = "hashicorp/aws", version = "~> 5.0" }
  }
}

provider "aws" {
  region = "us-east-1"
}

data "aws_ssm_parameter" "al2023_ami" {
  name = "/aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64"
}

resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = true
  enable_dns_hostnames = true
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main.id
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "us-east-1a"
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
}

resource "aws_route" "default" {
  route_table_id         = aws_route_table.public.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.igw.id
}

resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}

resource "aws_security_group" "web" {
  name        = "web-sg"
  description = "Allow HTTP"
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "web" {
  ami                    = data.aws_ssm_parameter.al2023_ami.value
  instance_type          = "t3.micro"
  subnet_id              = aws_subnet.public.id
  vpc_security_group_ids = [aws_security_group.web.id]

  user_data = <<-EOF
    #!/bin/bash
    dnf -y update
    dnf -y install nginx
    systemctl enable nginx
    systemctl start nginx
    echo "hello from terraform" > /usr/share/nginx/html/index.html
  EOF
}

output "web_url" {
  value = "http://${aws_instance.web.public_ip}"
}
```

---

如果你把你当前 Lab 的“这一步”贴出来（比如你是在 **S3 静态站点 / EC2 Nginx / ALB+ECS** 哪一种），我可以把上面的模板**精确对齐到你真实架构**：端口、role、ALB health check、ECS task SG、以及 NACL 的 ephemeral 端口规则都配到一模一样。
