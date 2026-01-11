
# CloudFormation Template Walkthrough (Line-by-line)

## Parameters

- `Parameters:`  
  定义“可配置输入”，部署栈时你可以填不同值（相当于模板的入参）。

- `  SecurityGroupDescription:`  
  定义一个参数名：SecurityGroupDescription（给安全组描述用）。

- `    Description: Security Group Description (Simple parameter)`  
  参数说明文字（只用于提示/文档，不影响资源行为）。

- `    Type: String`  
  参数类型是字符串。

---

- `  SecurityGroupPort:`  
  定义一个参数名：SecurityGroupPort（要开放的端口号）。

- `    Description: Simple Description of a Number Parameter, with MinValue and MaxValue`  
  参数说明：这是数字参数，并限制范围。

- `    Type: Number`  
  参数类型是数字。

- `    MinValue: 1150`  
  最小端口=1150（⚠️ 会导致 22/80/443 这种常用端口无法使用）。

- `    MaxValue: 65535`  
  最大端口=65535（端口上限）。

---

- `  InstanceType:`  
  定义一个参数名：InstanceType（EC2 实例规格）。

- `    Description: WebServer EC2 instance type (has default, AllowedValues)`  
  参数说明：EC2 类型，带默认值和可选列表。

- `    Type: String`  
  参数类型是字符串。

- `    Default: t2.small`  
  默认值是 t2.small（⚠️ 你之前失败的原因之一：可能不属于 Free Tier eligible）。

- `    AllowedValues:`  
  限制只能从下面列表里选（否则直接参数校验失败）。

- `      - t1.micro`  
  允许值之一：t1.micro（非常老的类型，很多场景不建议）。

- `      - t2.nano`  
  允许值之一：t2.nano（不常见）。

- `      - t2.micro`  
  允许值之一：t2.micro（传统免费层常见）。

- `      - t2.small`  
  允许值之一：t2.small（⚠️ 通常不是免费层）。

- `    ConstraintDescription: must be a valid EC2 instance type.`  
  如果你填了不在 AllowedValues 的值，报错时显示这句话。

---

- `  DBPwd:`  
  定义一个参数名：DBPwd（数据库密码）。

- `    NoEcho: true`  
  UI/日志不回显该值（保护敏感信息）。

- `    Description: The database admin account password (won't be echoed)`  
  参数说明：数据库管理员密码。

- `    Type: String`  
  参数类型是字符串。

- `  KeyName:`  
  定义一个参数名：KeyName（EC2 KeyPair 名称，用于 SSH）。

- `    Description: Name of an existing EC2 KeyPair to enable SSH access to the instances. Linked to AWS Parameter`  
  参数说明：必须是已存在 KeyPair，用来 SSH 登录实例。

- `    Type: AWS::EC2::KeyPair::KeyName`  
  参数类型是 AWS 特殊类型：只能选现有 KeyPair（控制台会给下拉选择）。

- `    ConstraintDescription: must be the name of an existing EC2 KeyPair.`  
  如果 KeyPair 不存在，报错时显示这句话。

---

- `  SecurityGroupIngressCIDR:`  
  定义一个参数名：SecurityGroupIngressCIDR（允许访问的 CIDR 范围）。

- `    Description: The IP address range that can be used to communicate to the EC2 instances`  
  参数说明：允许访问 EC2 的 IP 段。

- `    Type: String`  
  参数类型是字符串。

- `    MinLength: '9'`  
  最短长度限制（例如 0.0.0.0/0 长度=9）。

- `    MaxLength: '18'`  
  最长长度限制（适配常见 CIDR 字符串长度）。

- `    Default: 0.0.0.0/0`  
  默认对全世界开放（⚠️ 安全风险，建议改成你自己 IP/32）。

- `    AllowedPattern: (\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/(\d{1,2})`  
  正则校验格式必须像 x.x.x.x/x（⚠️ 只校验格式，不校验是否合理/可路由）。

- `    ConstraintDescription: must be a valid IP CIDR range of the form x.x.x.x/x.`  
  如果 CIDR 格式不对，报错时显示这句话。

---

- `  MyVPC:`  
  定义一个参数名：MyVPC（选择在哪个 VPC 里创建资源）。

- `    Description: VPC to operate in`  
  参数说明：要操作的 VPC。

- `    Type: AWS::EC2::VPC::Id`  
  参数类型：VPC ID（控制台会给可选 VPC 列表）。

---

- `  MySubnetIDs:`  
  定义一个参数名：MySubnetIDs（子网 ID 列表）。

- `    Description: Subnet IDs that is a List of Subnet Id`  
  参数说明：子网 ID 的列表。

- `    Type: "List<AWS::EC2::Subnet::Id>"`  
  参数类型：Subnet ID 列表（⚠️ 但你后面 Resources 并没有使用这个参数）。

---

- `  DbSubnetIpBlocks:`  
  定义一个参数名：DbSubnetIpBlocks（三个子网 CIDR 列表）。

- `    Description: "Comma-delimited list of three CIDR blocks"`  
  参数说明：逗号分隔的 3 个 CIDR。

- `    Type: CommaDelimitedList`  
  参数类型：逗号分隔列表（CloudFormation 会拆成数组）。

- `    Default: "10.0.48.0/24, 10.0.112.0/24, 10.0.176.0/24"`  
  默认提供三个 CIDR（⚠️ 必须落在你选的 VPC CIDR 范围内，否则创建 Subnet 会失败）。

---

## Resources

- `Resources:`  
  定义要创建的 AWS 资源。

---

- `  MyEC2Instance:`  
  定义一个资源逻辑名：MyEC2Instance（EC2 实例）。

- `    Type: AWS::EC2::Instance`  
  资源类型是 EC2 Instance。

- `    Properties:`  
  EC2 的具体配置。

- `      #we reference the InstanceType parameter`  
  注释：这里引用 InstanceType 参数。

- `      InstanceType: !Ref InstanceType`  
  实例规格来自 Parameters.InstanceType（你部署时选什么，这里就用什么）。

- `      KeyName: !Ref KeyName`  
  使用你传入的 KeyPair 名称（用于 SSH）。

- `      ImageId: ami-0742b4e673072066f`  
  固定 AMI（⚠️ 必须是 us-east-1 存在的 AMI；换 region 可能失效）。

- `      # here we reference an internal CloudFormation resource`  
  注释：这里引用模板内创建的资源。

- `      SubnetId: !Ref DbSubnet1`  
  把实例放到 DbSubnet1 子网里（这个子网在下面会创建）。

> ⚠️ 重要缺失：你创建了 MySecurityGroup，但 EC2 没绑定它；应加 `SecurityGroupIds: [!Ref MySecurityGroup]` 才会生效。

---

- `  MySecurityGroup:`  
  定义一个资源逻辑名：MySecurityGroup（安全组）。

- `    Type: AWS::EC2::SecurityGroup`  
  资源类型是 EC2 SecurityGroup。

- `    Properties:`  
  安全组配置。

- `      GroupDescription: !Ref SecurityGroupDescription`  
  安全组描述来自参数 SecurityGroupDescription。

- `      SecurityGroupIngress:`  
  定义入站规则列表。

- `        - CidrIp: !Ref SecurityGroupIngressCIDR`  
  允许访问的源 IP 段来自参数 SecurityGroupIngressCIDR。

- `          FromPort: !Ref SecurityGroupPort`  
  起始端口来自参数 SecurityGroupPort。

- `          ToPort: !Ref SecurityGroupPort`  
  结束端口也等于这个值（=只开放单个端口）。

- `          IpProtocol: tcp`  
  协议是 TCP。

- `      VpcId: !Ref MyVPC`  
  安全组创建在你选择的 VPC 里。

---

- `  DbSubnet1:`  
  定义一个资源逻辑名：DbSubnet1（子网 1）。

- `    Type: AWS::EC2::Subnet`  
  资源类型是 Subnet。

- `    Properties:`  
  子网配置。

- `      VpcId: !Ref MyVPC`  
  子网属于你选择的 VPC。

- `      # the select function allows us to select across a list`  
  注释：Select 从列表里按下标取值。

- `      CidrBlock: !Select [0, !Ref DbSubnetIpBlocks]`  
  CIDR 取 DbSubnetIpBlocks 列表的第 0 个（默认=10.0.48.0/24）。

---

- `  DbSubnet2:`  
  定义子网 2。

- `    Type: AWS::EC2::Subnet`  
  资源类型是 Subnet。

- `    Properties:`  
  子网配置。

- `      VpcId: !Ref MyVPC`  
  子网属于你选择的 VPC。

- `      # the select function allows us to select across a list`  
  注释：Select 从列表里按下标取值。

- `      CidrBlock: !Select [1, !Ref DbSubnetIpBlocks]`  
  CIDR 取列表第 1 个（默认=10.0.112.0/24）。

---

- `  DbSubnet3:`  
  定义子网 3。

- `    Type: AWS::EC2::Subnet`  
  资源类型是 Subnet。

- `    Properties:`  
  子网配置。

- `      VpcId: !Ref MyVPC`  
  子网属于你选择的 VPC。

- `      # the select function allows us to select across a list`  
  注释：Select 从列表里按下标取值。

- `      CidrBlock: !Select [2, !Ref DbSubnetIpBlocks]`  
  CIDR 取列表第 2 个（默认=10.0.176.0/24）。

