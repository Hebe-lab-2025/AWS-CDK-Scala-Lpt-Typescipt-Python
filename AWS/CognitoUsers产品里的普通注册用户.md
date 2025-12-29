“Cognito Users” 指的是：

✅ 什么是 Cognito 用户
	•	使用 Amazon Cognito 用户池（User Pool）注册/登录的用户
	•	通常是 你应用里的最终用户（App 用户），比如网站或手机 App 的账号
	•	不是 AWS 账号、不是 IAM 用户

🧩 对比理解

类型	用来登录	常见身份
Cognito 用户	你的 App（网站/手机应用）	普通应用用户
IAM 用户	AWS 控制台 / API	运维、开发人员
Root 用户	AWS 主账号	账号所有者

🧠 常见使用场景
	•	App 登录 / 注册
	•	短信验证码
	•	邮箱验证
	•	社交登录（Google / Apple / Facebook）
	•	忘记密码重置

👉 简单讲：

Cognito 用户就是 “你产品里的普通注册用户”，
IAM 用户是 “管理 AWS 的内部人员”。
