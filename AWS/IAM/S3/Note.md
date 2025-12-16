AWS S3 CLI 实战速查（含你刚做的全部步骤）
一、AWS S3 CLI 是什么

aws s3：高层封装，像 Linux 文件系统一样操作 S3（最常用）

aws s3api：低层 API，功能最全，用于精细控制

面试一句话：
### I use aws s3 for daily operations and aws s3api for advanced control.

二、常用 aws s3 命令（必须会）
1️⃣ 查看 Bucket
# aws s3 ls
# aws s3 ls s3://my-bucket

2️⃣ 创建 / 删除 Bucket
# aws s3 mb s3://my-bucket --region us-east-2

# aws s3 rb s3://my-bucket
# aws s3 rb s3://my-bucket --force


⚠️ Bucket 名规则：

全小写

全局唯一

3–63 字符

只能用 a-z 0-9 -

3️⃣ 上传文件（你做过）
# aws s3 cp index.html s3://my-bucket/index.html


上传目录：

# aws s3 cp ./Data s3://my-bucket/Data --recursive

4️⃣ 下载文件
# aws s3 cp s3://my-bucket/index.html index.html

5️⃣ 同步目录（生产常用）
# aws s3 sync ./local-dir s3://my-bucket

6️⃣ 删除对象
# aws s3 rm s3://my-bucket/file.txt
# aws s3 rm s3://my-bucket/data --recursive

7️⃣ 生成 Pre-signed URL（你刚做的重点）
# aws s3 presign s3://my-bucket/index.html --region us-east-2 --expires-in 3600


👉 用浏览器打开生成的 URL
👉 临时公开访问私有对象

三、你实际踩过的坑 & 正确理解（很重要）
❌ Bucket 名 InvalidBucketName

原因：

有大写字母

不符合 DNS 规则

✅ 解决：

全小写 + 随机后缀

❌ InvalidLocationConstraint

原因：

Bucket region ≠ CLI 默认 region

✅ 解决：

aws s3 mb s3://my-bucket --region us-east-2

❌ Pre-signed URL Region 错误

错误信息：

expecting 'us-east-2'


原因：

URL 用 us-east-1

Bucket 在 us-east-2

✅ 解决：

aws s3 presign s3://my-bucket/index.html --region us-east-2

四、常用 aws s3api（进阶）
查看 bucket region
aws s3api get-bucket-location --bucket my-bucket

精确创建 bucket（非 us-east-1）
aws s3api create-bucket \
  --bucket my-bucket \
  --region us-east-2 \
  --create-bucket-configuration LocationConstraint=us-east-2

五、Educative 项目完整流程（你已全部完成）

1️⃣ 创建 bucket
2️⃣ 本地创建 index.html
3️⃣ aws s3 cp 上传文件
4️⃣ aws s3 ls 验证
5️⃣ aws s3 presign 生成 URL
6️⃣ 浏览器访问
7️⃣ Claim Educative Certificate ✅

六、官方文档（收藏）

AWS CLI 总入口
https://docs.aws.amazon.com/cli/latest/index.html

aws s3 命令
https://docs.aws.amazon.com/cli/latest/reference/s3/

aws s3api 命令
https://docs.aws.amazon.com/cli/latest/reference/s3api/

S3 官方概念
https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html

七、面试速背 5 句（够用）

aws s3 is a high-level CLI for common S3 operations.

S3 bucket names must be globally unique and lowercase.

Pre-signed URLs provide temporary access to private objects.

The region in a pre-signed URL must match the bucket’s region.

I use aws s3api when I need fine-grained control.

八、下一步我可以帮你做的（强烈建议）

✅ 把这次 Educative 项目 写成简历一条 bullet（AWS / Cloudflare 都能用）
✅ 出一页 AWS CLI + S3 面试速背卡
✅ 帮你判断 这个证书放不放 LinkedIn / 简历


——————————

✅ 一、简历 Bullet（AWS / Cloudflare / New Grad 都合适）

Certification / Project（任选放哪一栏）

AWS S3 CLI Project (Educative)

### Created and managed Amazon S3 buckets using AWS CLI, following global naming and region constraints.

Uploaded and synchronized local files to S3 using aws s3 cp and aws s3 sync.

Generated pre-signed URLs to securely share private S3 objects with time-limited access.

Debugged common AWS CLI issues including invalid credentials and region mismatches.

👉 如果你想更偏 Cloudflare / Infra，用这版更好：

Practiced cloud storage operations using AWS S3 CLI, including bucket management, object uploads, and pre-signed URL generation.

Gained hands-on experience troubleshooting region-specific access issues and IAM-based authorization errors.

✅ 二、Certification 写法（最干净，不浮夸）

Educative Certification – AWS S3 Management with CLI

### Hands-on experience with AWS S3 operations, CLI workflows, and access control fundamentals.

（这一句非常安全，HR / 面试官都能接受）

✅ 三、LinkedIn 分享文案（低调版，推荐）

### Just completed an AWS S3 CLI hands-on project on Educative.
Practiced bucket management, file uploads, and pre-signed URL generation while troubleshooting real-world AWS region and credential issues.
Small project, but great practice for cloud fundamentals.

如果你 不想发 LinkedIn：完全没问题，放简历就够了。

✅ 四、面试一句话总结（非常重要）

### I’ve used AWS S3 through the CLI to create buckets, upload objects, and generate pre-signed URLs, and I’ve debugged common issues like region mismatches and credential errors.

这句话 Cloudflare / AWS / Backend / Infra 面试都通吃。




————————
# 记忆版本
「AWS S3 + IAM + CLI 面试速背卡（5 分钟版）」，这是可以直接临面前翻一遍的那种，不啰嗦、不浮夸。

AWS S3 + IAM + CLI 面试速背卡（New Grad / Cloudflare / AWS）
一、AWS S3（核心概念）
S3 是什么？

Object Storage（对象存储）

用于存文件（图片、日志、备份、静态网站等）

高可用、强一致性、全球可访问

面试一句话：
Amazon S3 is a highly durable object storage service used to store and retrieve data at scale.

S3 三个核心概念

Bucket：容器（全局唯一、有 region）

Object：文件（key + data + metadata）

Key：对象路径（类似文件路径）

二、S3 Bucket 规则（你已经踩过坑，面试加分）

必须 全小写

全局唯一

3–63 字符

只能用 a-z 0-9 -

Bucket 有 region，不能乱用

面试点：
S3 bucket names must follow DNS naming rules and are globally unique.

三、aws s3 常用 CLI（必背）
查看
aws s3 ls
aws s3 ls s3://my-bucket

创建 bucket
aws s3 mb s3://my-bucket --region us-east-2

上传 / 下载
aws s3 cp file.txt s3://my-bucket/file.txt
aws s3 cp s3://my-bucket/file.txt file.txt

同步（生产常用）
aws s3 sync ./local s3://my-bucket

删除
aws s3 rm s3://my-bucket/file.txt
aws s3 rb s3://my-bucket --force

四、Pre-signed URL（你刚做的重点）
是什么？

临时公开访问私有对象

使用生成者的 IAM 权限

有过期时间

命令
aws s3 presign s3://my-bucket/index.html --region us-east-2 --expires-in 3600

常见错误（你遇到的）

❌ Region 不一致 → 403 / XML Error
✅ Region 必须和 bucket 一致

面试一句话：
Pre-signed URLs are region-specific and time-limited.

五、IAM（和 S3 强相关）
IAM User 是什么？

AWS 里的 身份

有 access key / password

权限由 policy 决定

IAM Group 是什么？

多个 user 的集合

给 group 绑 policy，统一管理权限

面试一句话：
IAM groups simplify permission management by applying policies to multiple users.

常见 IAM + S3 权限

s3:ListBucket

s3:GetObject

s3:PutObject

六、aws s3 vs aws s3api（高频）
对比	aws s3	aws s3api
抽象	高	低
易用	高	低
功能	常用	全量
场景	日常操作	精细控制

面试标准回答：
I use aws s3 for daily tasks and aws s3api for advanced configurations.

七、你这个 Educative 项目 = 面试如何说？

一句话版本（推荐）

I used AWS S3 through the CLI to create buckets, upload objects, and generate pre-signed URLs, and I debugged common issues like region mismatches and credential errors.

稍微展开（30 秒）

I completed a hands-on AWS S3 CLI project where I managed buckets, uploaded files, and generated pre-signed URLs for temporary access. I also troubleshot real-world issues like invalid bucket names and region mismatches, which helped me understand how S3 and IAM work together.

八、Cloudflare / Infra 面试特别加分点

你可以自然提到：

region awareness

access control

temporary credentials / pre-signed access

debugging auth errors

👉 非常贴 Cloudflare 的网络 + infra 气质

下一步我可以直接帮你（强烈建议）

1️⃣ 把 S3 + IAM 和你 Amazon 实习 合并成一条 Infra Story
2️⃣ 给你一页 Cloudflare Networking + S3 对照表（DNS / CDN / Object Storage）
3️⃣ 做一轮 Cloudflare 技术模拟 Q&A（10 题高频）

————————

AWS CLI：aws s3 命令中文整理（重点版）
一、aws s3 是什么？

### aws s3 是 AWS CLI 提供的一组高层（high-level）S3 操作命令，适合日常使用，比如上传、下载、同步文件。

👉 如果你需要更底层、更精细的 API（比如逐个参数控制），用 aws s3api。

二、路径参数类型（非常重要）
1️⃣ LocalPath（本地路径）

表示本地文件或目录

可以是绝对路径或相对路径

./index.html
/home/user/data/

### 2️⃣ S3Uri（S3 路径）

表示 S3 bucket / 对象 / 前缀

必须以 s3:// 开头

# s3://my-bucket
# s3://my-bucket/index.html
# s3://my-bucket/folder/file.txt


📌 S3 中的“文件夹”其实是 key 前缀（prefix），用 / 分隔。

三、路径参数的顺序规则

大多数命令最多有 两个路径参数：

# aws s3 <command> <source> <destination>


第一个：源（source）

第二个：目标（destination）

如果只有一个路径参数 → 表示只对源做操作（如 ls, mb）

四、单文件 / 单对象操作

以下命令在 不加 --recursive 时，只处理单个文件或对象：

# cp（复制）

# mv（移动）

# rm（删除）

📌 结尾有没有 / 很重要
# aws s3 cp file.txt s3://bucket/
# → 上传为 s3://bucket/file.txt

# aws s3 cp file.txt s3://bucket/new.txt
# → 上传为 s3://bucket/new.txt

五、目录 / 前缀级操作

下面这些命令始终按“目录 / 前缀”处理：

命令	作用
# ls	列出 bucket / 目录
mb	创建 bucket
rb	删除 bucket
sync	同步目录

是否在路径结尾加 / 不影响结果

六、常用命令速查表（面试 + 实操）
📁 Bucket 操作
aws s3 ls                       # 列出所有 bucket
aws s3 mb s3://my-bucket        # 创建 bucket
aws s3 rb s3://my-bucket        # 删除空 bucket

📄 文件操作
aws s3 cp file.txt s3://bucket/
aws s3 cp s3://bucket/file.txt .
aws s3 mv file.txt s3://bucket/
aws s3 rm s3://bucket/file.txt

🔁 同步（最常用）
aws s3 sync ./local-dir s3://bucket/dir
aws s3 sync s3://bucket/dir ./local-dir

七、Include / Exclude 过滤规则（高频考点）
规则核心一句话：

后写的规则优先级更高

只上传 .jpg 文件：
aws s3 cp ./data s3://bucket/ --recursive \
  --exclude "*" \
  --include "*.jpg"

同时上传 .jpg 和 .txt
--exclude "*" --include "*.jpg" --include "*.txt"

⚠️ 常见坑

❌ 只写 --include 是没用的
✅ 必须先 --exclude "*"

八、presign：生成临时访问 URL（你之前用过）
aws s3 presign s3://bucket/index.html


生成 预签名 URL

默认有效期 1 小时

不需要 bucket 公开

常见错误：region 不一致

九、常见错误总结（你踩过的）
❌ InvalidBucketName

bucket 名不能有大写字母

不能太短

不能和别人重复

✅ 正确示例：

hebe-educative-2025

❌ InvalidLocationConstraint

us-east-1 特殊

CLI region 和 bucket region 不一致

✅ 解决：

aws configure
# 确认 region = us-east-2


或创建 bucket 时指定 region。

❌ AuthorizationQueryParametersError

presign 的 region 和 bucket region 不一致

📌 你的错误信息就是这个典型例子。

十、面试一句话总结（非常加分）

“aws s3 是一组高层 CLI 命令，适合文件级和目录级操作；路径分为 LocalPath 和 S3Uri；sync 常用于大规模数据同步；过滤规则是 exclude 先、include 后；需要注意 region 一致性问题。”

官方文档（建议收藏）

👉 https://docs.aws.amazon.com/cli/latest/reference/s3/
