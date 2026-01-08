🧠 核心意思一句话

AWS 考试有“暗号（cues）”，题目里的关键词就是在“暗示你用哪类服务”。

🔑 什么叫 “coded language（编码语言）”？

不是加密，而是：

- 👉 AWS 出题人用固定关键词，快速指向正确服务类型
- 👉 老手一眼就能“对号入座”
- 👉 新手容易忽略，开始乱想、乱设计

🧩 第一组暗号：Minimal operational overhead
原文含义

minimal operational overhead（最小运维负担）

考试翻译

- 👉 “我不想管服务器”

正确联想
```
✔ Serverless / Fully Managed 服务：

- AWS Lambda（不用管服务器）

- Amazon S3（托管存储）

- DynamoDB（托管 NoSQL）
```
错误联想（扣分）

- ❌ EC2 + 自己装数据库

- ❌ 自己 patch OS

- ❌ 自己管 scaling / backup

- 📌 一看到这个词，EC2 基本就该被排除了
