## 🔍 先把“事件结构”对齐：你现在的解析要匹配哪种 API Gateway？

API Gateway 常见两种事件长相（**你 Lambda 里必须兼容/明确选一种**）：

### A) REST API (v1) 常见字段

* Query：`event["queryStringParameters"]`
* Path params：`event["pathParameters"]`
* Body：`event["body"]`（字符串；可能 `isBase64Encoded`）

### B) HTTP API (v2) 常见字段

* Query：`event["queryStringParameters"]`
* Path params：`event["pathParameters"]`
* Body：`event["body"]`（字符串）
* 额外：`event["requestContext"]["http"]["method"]`、`event["rawPath"]`

你之前的写法（`queryStringParameters`）对 **v1/v2 都 OK**，但你如果要更稳（尤其是 body），建议用下面这套“统一解析器”。

---

## ✅ 一个“对照代码用”的标准解析模板（query / path / body 全匹配）

把这段直接用到你的 Lambda 里：

```python
import json
import base64

def parse_request(event):
    event = event or {}

    # method: v2优先，其次 v1
    method = (
        (event.get("requestContext") or {}).get("http", {}).get("method")
        or event.get("httpMethod")
        or "GET"
    )

    # path: v2优先，其次 v1
    path = event.get("rawPath") or event.get("path") or ""

    # query: v1/v2 都是这个字段
    query = event.get("queryStringParameters") or {}

    # path params: v1/v2 都是这个字段
    path_params = event.get("pathParameters") or {}

    # body: 可能是字符串，也可能是 None
    body_raw = event.get("body")
    if body_raw and event.get("isBase64Encoded"):
        body_raw = base64.b64decode(body_raw).decode("utf-8")

    body_json = None
    if body_raw:
        try:
            body_json = json.loads(body_raw)
        except json.JSONDecodeError:
            body_json = None  # 非 JSON，就保持 None（或你也可以返回 400）

    return method, path, query, path_params, body_raw, body_json
```

### 🔍 你怎么“确认匹配”？

拿你 Lambda 里实际用的字段对照：

* 如果你用 `queryStringParameters` 做筛选：✅ OK（GET params）
* 如果你用 body 来创建/更新：必须 `json.loads(event["body"])` ✅（并处理 base64）
* 如果你想用 `/courses/{id}`：必须读 `pathParameters["id"]` ✅

---

## 🧪 最小 curl 测试（先验证后端，再测前端）

> 你如果线上已经有 API Gateway URL，就用那个；如果你本地 `sam local start-api`，就用 `http://127.0.0.1:3000`

### 1) 先测 GET query（最小）

```bash
curl -i "http://127.0.0.1:3000/courses?courseId=CS101"
```

### 2) 再测 GET path param（标准 REST）

```bash
curl -i "http://127.0.0.1:3000/courses/CS101"
```

### 3) 最后测 POST body（创建）

```bash
curl -i -X POST "http://127.0.0.1:3000/courses" \
  -H "Content-Type: application/json" \
  -d '{"courseId":"CS201","title":"Algorithms","credits":4}'
```

**你想更快确认解析对不对**：
先在 Lambda 里临时加一行打印（CloudWatch 或 sam local logs）：

```python
print({"method": method, "path": path, "query": query, "pathParams": path_params, "body": body_raw})
```

---

## 🧠 改成更标准 REST（body vs params）版本

### ✅ 推荐 API 设计（清晰、可扩展）

* `GET /courses`：列表（用 query 做过滤/分页）

  * `GET /courses?dept=CS&limit=20&cursor=...`
* `GET /courses/{courseId}`：单条（**用 path param**）
* `POST /courses`：创建（**用 JSON body**）
* `PUT /courses/{courseId}` 或 `PATCH /courses/{courseId}`：更新（**body**）
* `DELETE /courses/{courseId}`：删除

---

## ✍️ 参考实现：一个 Lambda 同时支持 GET list / GET by id / POST（标准版）

> 你 DynamoDB key 我先按“单主键 courseId”写；如果你实际是 `pk+sk`，我也给你改成 Query 模式。

```python
import os, json, base64
import boto3

dynamodb = boto3.resource("dynamodb")
table = dynamodb.Table(os.environ.get("COURSES_TABLE", "Courses"))

def resp(code, obj):
    return {
        "statusCode": code,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps(obj, default=str),
    }

def parse_request(event):
    event = event or {}
    method = ((event.get("requestContext") or {}).get("http", {}).get("method")
              or event.get("httpMethod") or "GET")
    path = event.get("rawPath") or event.get("path") or ""
    query = event.get("queryStringParameters") or {}
    path_params = event.get("pathParameters") or {}

    body_raw = event.get("body")
    if body_raw and event.get("isBase64Encoded"):
        body_raw = base64.b64decode(body_raw).decode("utf-8")

    body_json = None
    if body_raw:
        try:
            body_json = json.loads(body_raw)
        except json.JSONDecodeError:
            body_json = None

    return method.upper(), path, query, path_params, body_json

def lambda_handler(event, context):
    method, path, query, path_params, body = parse_request(event)

    # ---- GET /courses/{courseId} ----
    # 你的 API Gateway/SAM 路由要把 {courseId} 绑定到 pathParameters
    course_id = path_params.get("courseId") or path_params.get("id")
    if method == "GET" and course_id:
        r = table.get_item(Key={"courseId": course_id})
        item = r.get("Item")
        return resp(200, item) if item else resp(404, {"message": "Not found", "courseId": course_id})

    # ---- GET /courses (list) ----
    if method == "GET" and (path.endswith("/courses") or path == "/courses"):
        # 标准：GET 只用 query 做过滤；最小 demo 先 scan + limit
        limit = int(query.get("limit", "20"))
        r = table.scan(Limit=limit)
        return resp(200, {"items": r.get("Items", []), "count": r.get("Count", 0)})

    # ---- POST /courses ----
    if method == "POST" and (path.endswith("/courses") or path == "/courses"):
        if not body:
            return resp(400, {"message": "Missing JSON body"})
        # 最小校验
        for f in ["courseId", "title"]:
            if f not in body:
                return resp(400, {"message": f"Missing field: {f}"})

        table.put_item(Item=body)  # 注意：Put 会覆盖同主键已有 item
        return resp(201, {"message": "Created", "courseId": body["courseId"]})

    return resp(404, {"message": "Route not found", "method": method, "path": path})
```

### 对应的 SAM 路由（你 template.yaml 里要这样配）

* `GET /courses`
* `GET /courses/{courseId}`
* `POST /courses`

（你要的话我可以直接把 `Events:` 三条完整写给你，包含同一个 Lambda 多路由，或拆成两个 Lambda 更清晰。）

---

## 你现在要我“对照 Lambda 代码确认解析是否匹配”

把下面两样贴出来任意一个就行（不用全贴）：

1. 你的 Lambda handler 里解析 `event` 的那几行（query/path/body）
2. 你的 `template.yaml` 里 `Events:`（Path/Method）那段

我就能明确告诉你：**你现在的前端请求方式**（query 还是 body、GET 还是 POST、path param 叫啥）是否和 Lambda 解析 **完全对齐**，哪里会导致 400/404/500。
