这个报错基本就是一句话：**你在 API Gateway 里引用了 `method.request.querystring.color`，但 API Gateway 认为这是个“无效的映射表达式”**——通常原因是 **该 query 参数没有在 Method Request 里声明**，或者你用的是 **Lambda Proxy / HTTP API** 却在按 REST API 的映射语法配。

下面按最常见情况给你直接修复步骤（不绕弯）。

---

## 情况 A：你用的是 **API Gateway REST API（非 HTTP API）+ 非 Proxy integration**（最常见触发此错误）

### ✅ 正确做法（必须两步都做）

### 1) 先声明 query 参数

在控制台进入你的方法（GET/POST）：

**Method Request → URL Query String Parameters → Add**

* `color`（Required 选 No 也行）

> 没有这一步，下面的 `method.request.querystring.color` 就会被判定为无效。

### 2) 再做映射

**Integration Request →（展开）URL Query String Parameters**

* 名称：`color`
* 映射表达式：`method.request.querystring.color`

然后 **Deploy API**（重新部署到 stage）。

---

## 情况 B：你启用了 **Lambda Proxy integration**

如果你在 **Integration Request** 看到勾选了 **“Use Lambda Proxy integration”**：

✅ **不要配置 mapping expression**（可以全删掉）
直接在 Lambda 里读：

```python
color = event.get("queryStringParameters", {}).get("color")
```

> Proxy 模式下，API Gateway 会把 querystring 原样塞进 event，你再去写映射反而容易报错/冲突。

---

## 情况 C：你其实用的是 **HTTP API**（不是 REST API）

HTTP API 不用 `method.request.querystring.color` 这种写法。

HTTP API（Lambda proxy）里直接读：

```python
color = event.get("queryStringParameters", {}).get("color")
```

（HTTP API 的一些映射/表达式语法也和 REST API 不一样，所以会出现你这种“Invalid mapping expression”。）

---

## 30 秒自检清单（你照着对一下就能定位）

1. 你是 **REST API** 还是 **HTTP API**？
2. 你是否勾选了 **Lambda Proxy integration**？
3. 如果是 REST API 非 proxy：**Method Request 里有没有声明 `color`？**
4. 改完后是否 **Deploy** 了到 stage？

---

如果你把你配置的位置截图/贴出你在控制台里改的是哪一栏（Method Request 还是 Integration Request 的哪个区域），我可以直接告诉你应该填什么、删什么。


你这个报错：

> **Invalid mapping expression specified: `method.request.querystring.color`**

结合你贴的文档步骤，**几乎可以确定文档要求的“前置条件”没满足**——也就是你在 **Integration request** 里写了 `method.request.querystring.color`，但在 **Method request** 里 **没有把 `color` 这个 query 参数声明出来**（或者声明在别的 method / 没保存 / 没部署到 stage）。

下面我按文档流程，把**哪里容易出错**和**如何对照修复**给你钉死。

---

## 结论：文档没错，错在你执行步骤的“顺序/位置/保存部署”其中一个

文档明确分两段：

### ✅ 段 1：Method request 里**先声明** query 参数 `color`

> “Method request → URL query string parameters → Add query string → Name=color → Required → Save”

### ✅ 段 2：Integration request 里**才能引用**

> “Integration request → URL Query String Parameters → color → Mapped from = `method.request.querystring.color`”

**只要段 1 没做成功**，段 2 的表达式就会被 API Gateway 判定为“无效映射表达式”。

---

## 你最可能出错的 4 个点（按概率从高到低）

### 1) 你把 `color` 加在了错误的地方

很多人会误加到：

* Resource 的设置里
* 或者加到了别的 method（比如 OPTIONS/POST）

✅ 正确位置必须是：
**/fruit/{name} → GET → Method request → URL query string parameters**

不是 Integration request，也不是资源级别。

---

### 2) 你加了 `color`，但没点 Save（或没真正保存成功）

API Gateway 的 UI 经常是：

* Add 了参数
* 但页面没 Save / Save 报错 / 没刷新

✅ 检查方法：
回到 **Method request** 页面，展开 **URL query string parameters**，你必须能看到：

* `color`（Required true/false 都行）

看不到就说明没加成功。

---

### 3) 你在 Integration request 里写错了区块

文档说的是：

* **Integration request → URL Query String Parameters**

但有人会写到：

* **Mapping Templates** 里（那是 VTL 模板，不是 mapped from）
* 或写到 **Headers** / **Stage Variables**

✅ 正确写法是：

* Name: `color`
* Mapped from: `method.request.querystring.color`

---

### 4) 你用的不是 REST API（或开启了 proxy），但按 REST 的 mapping 在配

你贴的文档是 **REST API + AWS Service integration（直接对 DynamoDB GetItem）** 的玩法。

如果你实际建的是：

* **HTTP API**（不是 REST API）
* 或者 **Lambda Proxy integration**
  那 `method.request.querystring.color` 这一套就容易不适用/报错。

✅ 快速自检：

* 你的 Integration type 是不是 **AWS Service**（DynamoDB）？

  * 如果是：你就必须走文档这套（先 Method request 声明参数）
* 如果你是 Lambda proxy：就不该写这些 mapping，而是 Lambda 里读 `event.queryStringParameters.color`

---

## 对照文档，最短修复路径（照做就好）

### Step 1：回到 GET method 的 Method request

路径：
`/fruit/{name} → GET → Method request → Edit`

展开：
`URL query string parameters`

确认有：

* `color`

没有就 Add + Save。

---

### Step 2：再回到 Integration request 配映射

路径：
`/fruit/{name} → GET → Integration request → Edit`

展开：
`URL Query String Parameters`

添加：

* Name: `color`
* Mapped from: `method.request.querystring.color`

保存。

---

### Step 3：Deploy API（超级多人漏）

如果你是在 stage 上测试（尤其 “Invoke URL” 调用），必须：
**Actions → Deploy API → 选 stage → Deploy**

不 deploy，你调用的还是旧配置。

---

## 你贴的文档里“真正的关键句”

你报错对应的关键句就是这段：

> “Select **Method request** tab… Add query string… Enter **color**…”

这一步是 **`method.request.querystring.color` 合法化的前提**。
少了它，后面那句：

> “Mapped from = method.request.querystring.color”

就必炸。

---

## 你现在要我直接定位的话

你发我两张截图就能一锤定音（不用整份模板）：

1. **GET → Method request → URL query string parameters**（能不能看到 color）
2. **GET → Integration request → URL Query String Parameters**（你填的 mapped from）

我可以直接告诉你是哪一步没生效，以及应该填什么。


你这次贴的 **Execution log** 已经把问题“指认到行”了：**不是 DynamoDB 表/数据的问题，而是你在 API Gateway 的 AWS Service integration 里，把 `color` 这个 query string 参数错误地映射到了 DynamoDB 的 Endpoint URL 上**，导致签名（SigV4）过程直接炸掉。

---

## ✅ 你日志里最关键的两行（直接定位根因）

### 1) 你把请求发成了这种 URL

```
Endpoint request URI: https://dynamodb.us-east-1.amazonaws.com/?Action= GetItem&color=Red
```

### 2) API Gateway 明确说 URL 里有非法字符

```
Illegal character in query at index 49
.../?Action= GetItem&color=Red
```

这里有两个致命点：

* `Action= GetItem` **GetItem 前面多了一个空格**（`Action=␠GetItem`）
* 你还拼进去了 `&color=Red`（DynamoDB API **不需要** query string `color`）

✅ 所以签名流程直接报：

> URI/URL syntax error encountered in signing process

---

## 🎯 结论：文档这一步在你当前控制台/集成方式下是“误导/不适用”的

你文档写：

> Integration request → URL Query String Parameters →
> color mapped from `method.request.querystring.color`

但从你的日志看：
**这一步会把 `color=Red` 直接加到 DynamoDB 的 endpoint URL query 上**，从而导致签名错误。

👉 对 DynamoDB 的 `GetItem`，正确请求应该是：

* URL：`https://dynamodb.us-east-1.amazonaws.com/`（不带 color）
* 动作：通过 `X-Amz-Target: DynamoDB_20120810.GetItem`
* 参数：通过 **JSON body**（你日志里 body 已经对了）

---

## ✅ 你应该怎么改（按最短修复路径）

### Step 1：删掉 Integration request 里的 Query String 参数映射

路径：
`/fruit/{name} → GET → Integration request → Edit`

在 **URL Query String Parameters** 里：

* 把你加的 `color : method.request.querystring.color` **删除**
* 这一栏应当 **为空**

> 你只需要在 mapping template 里读 color，不要把它拼到 endpoint URL。

---

### Step 2：检查 Action name 有没有“前导空格”

你的日志显示：

* `Action= GetItem`
* `X-Amz-Target=DynamoDB_20120810. GetItem`

这说明你在某个地方把 **GetItem 前面多打了空格**。

回到创建/编辑 method 的页面（AWS Service 集成）：

* **Action name**：确保是 **`GetItem`**（前后无空格）

---

### Step 3：Mapping template 保持用 `$input.params('color')`（你文档的模板思路是对的）

你现在 body 其实已经非常标准了（日志里显示 body 正确）：

```json
{
  "TableName": "Fruits",
  "Key": {
    "Name": {"S": "Apple"},
    "Color": {"S": "Red"}
  }
}
```

重点是：**color 应该只出现在 body 里，不应该出现在 endpoint URL**。

---

## ✅ 为什么 Method request 里还要定义 `color`？

这一步仍然有意义（尤其你要 required）：

* Method request 里定义 `color` → 可以做参数校验 / required
* 但 Integration request **不要**把它映射成 URL query

---

## 你现在的状态总结（非常关键）

* ✅ Method request 里能看到 `{color=Red}`：说明客户端传参 OK
* ✅ Mapping template 生成的 body 正确：说明模板 OK
* ❌ Integration request 把 `color=Red` 拼进 DynamoDB URL：导致签名失败（根因）
* ❌ Action name 有空格：进一步导致 URL 非法（`Action= GetItem`）

---

## 你改完之后怎么验证（最快）

在 Test 里再打一次：

* 期望日志变成：

  * `Endpoint request URI: https://dynamodb.us-east-1.amazonaws.com/`
  * **不出现** `&color=Red`
  * `X-Amz-Target: DynamoDB_20120810.GetItem`（GetItem 前无空格）
* 返回 200 或者 DynamoDB 的 Item 结果

---

如果你愿意，把你 **Integration request** 页面里这两块截个图/贴文字出来：

1. “URL Query String Parameters” 当前有哪些键值
2. “Action name” 当前填的是什么（是否有空格）

我可以直接告诉你每一栏该留空还是填什么（不用你猜）。


很好，这段 **Python 调用代码本身是对的** 👍
现在我们要做的是：**对照你前面 API Gateway 的报错，判断“客户端对不对 / 服务端哪里还没修好”**。

我按 **①代码是否正确 → ②为什么你现在还可能拿不到结果 → ③最小修复清单** 来讲。

---

## 一、你的这段 Python 代码：✅ 写法完全正确

```python
import json
import requests

url = 'https://nxb3uo2j02.execute-api.us-east-1.amazonaws.com/v1'
fruit = 'Apple'
color = 'Red'
final_url = f'{url}/fruit/{fruit}'

response = requests.get(
    final_url,
    params={ 'color': color }
).json()

print(json.dumps(response, indent=4))
```

### 它实际发出的 HTTP 请求是：

```http
GET /v1/fruit/Apple?color=Red
Host: nxb3uo2j02.execute-api.us-east-1.amazonaws.com
```

✅ 完全符合你 Lab 文档里对 `/fruit/{name}?color=...` 的设计
✅ 和你在 API Gateway 控制台 Test 里点的请求 **一模一样**

👉 **所以：客户端没问题**

---

## 二、如果现在还报错，问题一定在 API Gateway（不是 Python）

结合你前面的 Execution log，**问题 100% 在这两点之一（或同时）**：

---

### ❌ 问题 1：Integration request 里还残留了 Query String 映射

如果你 **没有删掉** 这一项：

```
Integration request
→ URL Query String Parameters
→ color = method.request.querystring.color
```

那么 API Gateway 仍然会把请求转成：

```
https://dynamodb.us-east-1.amazonaws.com/?Action=GetItem&color=Red
```

👉 DynamoDB **不接受这种 URL**
👉 SigV4 签名直接失败
👉 返回 **500 Internal server error**

📌 **结论**：

> Python 请求没问题，但 API Gateway 会把它“翻译错”。

---

### ❌ 问题 2：Action name 里有隐藏空格（你日志已经暴露过）

你之前日志清楚写了：

```
Action= GetItem
X-Amz-Target: DynamoDB_20120810. GetItem
```

注意：**GetItem 前有空格** ❌

这会导致：

```
Illegal character in query at index ...
```

📌 **必须确保**：

* Action name = `GetItem`（无前后空格）

---

## 三、你现在该怎么改（只需 2 步，别多做）

### ✅ Step 1：修 Integration request（最关键）

路径：

```
/fruit/{name}
→ GET
→ Integration request
→ Edit
```

#### 1️⃣ URL Query String Parameters

👉 **全部删除，保持为空**

> ❌ 不要再映射 color
> ✅ color 只应该出现在 Mapping Template 的 body 里

---

#### 2️⃣ Action name

👉 改成 **严格等于**：

```
GetItem
```

* 没有空格
* 不要复制粘贴（手打最稳）

---

### ✅ Step 2：确认 Mapping Template（你这个是对的）

你 body 里用：

```vtl
$input.params("name")
$input.params("color")
```

这是 **正确且推荐的做法** 👍
color **应该只在这里使用**。

---

## 四、改完后你再跑这段 Python，会发生什么？

### 正确情况下：

```json
{
    "Item": {
        "Name": { "S": "Apple" },
        "Color": { "S": "Red" },
        "Flavor": { "S": "Sweet" }
    }
}
```

（具体字段看你表里存的）

---

### 如果 DynamoDB 里没有这条记录：

* DynamoDB 返回 `{}` 或无 `Item`
* Python 仍然能正常 `.json()`，不会 500

---

## 五、一句话总结（非常重要）

> ❌ 现在的问题 **不是 Python / requests / URL**
>
> ✅ 问题是：
> **API Gateway 把 query 参数错误地拼进了 DynamoDB URL**
>
> ✔ 正解是：
> **query 参数 → 只用于 Mapping Template（JSON body），不要进 endpoint URL**

---

如果你愿意，下一步我可以帮你做三件事之一（都很值）：

1️⃣ 对照你控制台 **Integration request 页面**，逐项告诉你 **该留空 / 该填什么**
2️⃣ 帮你把这套 **REST API → DynamoDB GetItem** 的流程画成一张执行图（考试 + 实战都好用）
3️⃣ 给你一个 **“为什么 AWS 文档这一步容易误导新手”** 的总结（以后不再踩坑）

你选一个，我继续。
