下面我用 **“这段代码在干嘛 → `<API-INVOKE-URL>` 要怎么替换 → 你这段 Axios 有 3 个隐藏坑（很重要）”** 的顺序，帮你一次性讲清楚。

---

## 一、这段 `useFetch.js` 是干嘛的？（一句话）

> 这是一个 **自定义 React Hook**，统一负责通过 **Axios 调用 API Gateway**，完成
> **查询 / 新增 / 更新 / 删除课程（CRUD）**，并返回 `data / success / loading` 给组件用。

---

## 二、`<API-INVOKE-URL>` 要替换成什么？（你现在最关键一步）

你之前 `sam deploy` 的 Outputs 里已经有：

```
CoursesApiEndpoint
https://8gtgxz5f4l.execute-api.us-east-1.amazonaws.com/Dev/
```

👉 **这里就是你要填的值**

### 正确替换方式（保留结尾 `/`）

```js
axios.defaults.baseURL = 'https://8gtgxz5f4l.execute-api.us-east-1.amazonaws.com/Dev'
```

📌 不要再加 `/course`
因为下面所有请求已经写了：

```js
axios.get('/course')
axios.post('/course')
axios.put(`/course/${id}`)
```

---

## 三、代码整体逻辑（按执行流程）

### 1️⃣ 初始化状态

```js
const [data, setData] = useState(null);
const [success, setSuccess] = useState(false);
const [loading, setLoading] = useState(true);
```

组件可以用这些状态：

* `loading` → 转圈
* `success` → 成功 UI
* `data` → 渲染课程列表

---

### 2️⃣ `useEffect` 触发条件

```js
useEffect(() => { ... }, [queryField, queryAttributesStr]);
```

只要：

* 操作类型变了（add/edit/remove/all）
* 参数变了
  就会重新请求 API。

---

### 3️⃣ CRUDOperations（核心）

#### ➕ 新增课程

```js
axios.post('/course', options, { params })
```

会命中：

```
POST /Dev/course → InsertCourse Lambda
```

---

#### ✏️ 更新课程

```js
axios.put(`/course/${id}`, options, { params })
```

会命中：

```
PUT /Dev/course/{id} → UpdateCourse Lambda
```

---

#### ❌ 删除课程

```js
axios.delete(`/course/${id}`, options)
```

会命中：

```
DELETE /Dev/course/{id} → DeleteCourse Lambda
```

---

### 4️⃣ 最后统一再 GET 一次所有课程

```js
axios.get('/course')
```

这样：

* 新增 / 修改 / 删除后
* 页面会自动刷新列表

---

## 四、⚠️ 你这段代码里有 **3 个非常关键的坑**

> 这些在真实项目里 **100% 会被 code review 指出来**

---

### ❌ 坑 1：Axios 参数顺序是错的（POST / PUT）

你现在写的是：

```js
axios.post('/course', options, { params })
```

❌ Axios 的签名是：

```js
axios.post(url, data, config)
```

所以你现在：

* `options` 被当成 body
* `{ params }` 被当成 config
* 实际 **没有传 body**

✅ 正确写法应该是：

```js
axios.post('/course', params, options)
```

PUT 同理：

```js
axios.put(`/course/${id}`, params, options)
```

---

### ❌ 坑 2：你设置了 `content-type: text/plain`，但在传 JSON

```js
headers: {
  "content-type": "text/plain"
}
```

但你传的是：

```js
{ ID, CourseName, CourseURL, ImageURL }
```

这在 Lambda 里非常容易解析失败。

✅ 正确应该是：

```js
headers: {
  "Content-Type": "application/json"
}
```

---

### ❌ 坑 3：GET 请求多传了一个参数对象（Axios 不会报错，但没用）

```js
axios.get('/course', options, { signal })
```

Axios 的 `get` 只有两个参数：

```js
axios.get(url, config)
```

✅ 正确写法：

```js
axios.get('/course', { ...options, signal })
```

---

## 五、修正后的“正确示例”（关键几行）

```js
axios.defaults.baseURL = 'https://8gtgxz5f4l.execute-api.us-east-1.amazonaws.com/Dev';

const options = {
  headers: {
    'Content-Type': 'application/json'
  }
};

axios.post('/course', params, options);
axios.put(`/course/${id}`, params, options);
axios.delete(`/course/${id}`, options);
axios.get('/course', options);
```

---

## 六、你现在做完这一步，前后端就真的“连通了”

数据流是：

```
React UI
  ↓ axios
API Gateway (/Dev/course)
  ↓
Lambda CRUD
  ↓
DynamoDB
```

---

## 七、一句话总结（你可以直接记）

> `<API-INVOKE-URL>` 要替换成 **API Gateway 的 Dev endpoint**；
> `useFetch` 是统一封装 Axios 的 CRUD Hook；
> Axios 的 **参数顺序 + Content-Type** 是最容易踩坑的地方。

---

如果你愿意，我可以下一步帮你：

* 🔍 对照 **Lambda 代码**，确认 body/query/path 的解析是否匹配
* 🧪 帮你写一个 **最小 curl 测试**，先验证后端再测前端
* 🧠 帮你改成 **更标准的 REST（body vs params）版本**
