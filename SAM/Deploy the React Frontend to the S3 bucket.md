我把 **“Deploy the React Frontend to the S3 bucket”** 这一步，**按你现在这个 lab 的实际操作顺序**，给你一版**可直接照做 + 不踩坑**的说明。

---

## 一、这一 task 到底在做什么？（一句话）

> **把 React build 生成的静态文件（index.html 等）上传到 S3，
> 然后通过 S3 的 Static Website URL 访问整个前端页面。**

---

## 二、你现在需要做的 3 件事（顺序很重要）

### ✅ 第 1 步：先改前端里的 API 地址（你已经知道这一步）

在 `hooks/useFetch.js` 里：

```js
axios.defaults.baseURL = '<API-INVOKE-URL>'
```

替换成你之前 **CloudFormation Outputs** 里的值，例如：

```js
axios.defaults.baseURL = 'https://8gtgxz5f4l.execute-api.us-east-1.amazonaws.com/Dev'
```

> ⚠️ 不要加 `/course`
> ⚠️ 末尾 `/Dev` 要保留

---

### ✅ 第 2 步：生成 React 的 build directory（关键概念）

> **S3 静态网站 ≠ React 源码**
> **S3 需要的是 build 目录里的 index.html**

在前端项目根目录运行：

```bash
npm install
npm run build
```

运行完之后你会看到：

```
build/
├── index.html   👈 S3 静态网站必须要的
├── static/
└── asset-manifest.json
```

这里的 **build/** 就是文档里反复说的 **build directory**。

---

### ✅ 第 3 步：把 build 目录上传到 S3（最关键的命令）

假设你的 bucket 叫（例子）：

```
sam-web-bucket-097285037130
```

运行：

```bash
aws s3 sync build/ s3://sam-web-bucket-097285037130 --delete
```

#### 这条命令在干嘛？

* `build/`：本地 React build 目录
* `s3://sam-web-bucket-...`：你创建的 **web content bucket**
* `--delete`：保证 S3 内容和本地 build 完全一致（防止旧文件残留）

---

## 三、怎么访问你的网站？（不是 API URL）

> ⚠️ **不是 API Gateway URL**

### 正确方式（S3 Website Endpoint）

1. 打开 **AWS Console → S3**
2. 点击你的 **web bucket**
3. 打开 **Properties**
4. 找到 **Static website hosting**
5. 复制 **Bucket website endpoint**

格式一般是：

```
http://sam-web-bucket-097285037130.s3-website-us-east-1.amazonaws.com
```

👉 在浏览器新 tab 打开
👉 你应该能看到 React 页面

---

## 四、最常见 4 个问题（你如果遇到，直接对号入座）

### ❌ 1️⃣ 打开是 403 Forbidden

检查：

* Bucket Policy 是否允许 `s3:GetObject`
* Block Public Access 是否 **全部关闭**

---

### ❌ 2️⃣ 打开是 404 Not Found

检查：

* `build/index.html` 是否真的上传了
* S3 的 **Index document** 是否是 `index.html`

---

### ❌ 3️⃣ 页面能打开，但点按钮没反应 / Network Error

检查：

* `useFetch.js` 里的 `<API-INVOKE-URL>` 是否替换
* API Gateway 是否返回 200（打开浏览器 DevTools → Network）

---

### ❌ 4️⃣ 页面空白

检查：

* React build 是否成功
* 控制台是否有 JS 报错（CORS / API 错误）

---

## 五、你现在整个系统的最终形态（心里要有这张图）

```
Browser
   |
   |  HTTP (S3 Website)
   v
S3 Static Website (index.html, js, css)
   |
   |  Axios
   v
API Gateway (/Dev/course)
   |
   v
Lambda CRUD
   |
   v
DynamoDB
```

---

## 六、一句话总结（可以直接背）

> 先用 `npm run build` 生成 React 的 build 目录，
> 再用 `aws s3 sync build/ s3://<WEB-BUCKET>` 上传，
> 最后通过 **S3 Static Website endpoint** 访问前端页面。

如果你愿意，我可以下一步帮你：

* 🔍 对着浏览器 Network 面板一起查一次 API 调用
* 🧠 把 **S3 静态网站 vs CloudFront** 的区别用 5 行讲清楚
* 🧪 给你一套「403 / 404 / CORS」排错清单
