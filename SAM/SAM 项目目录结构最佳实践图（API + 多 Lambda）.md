## 🧠 SAM 项目目录结构最佳实践图（API + 多 Lambda）

```text
sam-courses-app/
├── template.yaml                 # ✅SAM/CFN 入口（Functions / API / Outputs）
├── samconfig.toml                # ✅sam deploy --guided 生成的部署参数（推荐提交到repo）
├── README.md
├── events/                       # ✅本地调试事件样例（sam local invoke -e）
│   ├── getCourses.json
│   └── getCourseById.json
├── src/                          # ✅函数代码根目录（CodeUri 指向这里，或更细到每个函数）
│   ├── get_courses/              # ✅每个 Lambda 一个文件夹（强烈推荐）
│   │   ├── app.py                # ✅handler 文件（Handler: app.lambda_handler）
│   │   └── requirements.txt      # ✅该函数独有依赖（sam build 会打包）
│   └── common/                   # ✅共享代码（utils / clients / models）
│       ├── __init__.py
│       └── dynamo.py
├── tests/                        # ✅单元测试（pytest/jest）
│   └── test_get_courses.py
└── .aws-sam/                     # sam build 输出（✅不提交git）
    └── build/...
```

### ✅ 关键原则（不踩坑）

* **每个 Lambda 单独目录**：依赖、代码、handler 清晰
* `events/`：本地 invoke 用的 event JSON 固化下来
* `.aws-sam/`：只要 build 就会生成，**不要提交**
* 共享代码放 `src/common/`，通过 Python package 引用（注意 `__init__.py`）

---

## ✍️ `getCourses` Lambda + `template.yaml` 对应示例（可直接跑）

### 1) `src/get_courses/app.py`

支持三种调用：

* `/courses?courseId=CS101` → GetItem（单条）
* `/courses?pk=DEPT#CS&skPrefix=COURSE#` → Query（推荐）
* `/courses` → Scan（demo）

```python
import os
import json
import boto3
from boto3.dynamodb.conditions import Key

dynamodb = boto3.resource("dynamodb")
TABLE_NAME = os.environ.get("COURSES_TABLE", "Courses")
table = dynamodb.Table(TABLE_NAME)

def _resp(code, obj):
    return {
        "statusCode": code,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps(obj, default=str),
    }

def lambda_handler(event, context):
    q = (event or {}).get("queryStringParameters") or {}

    # 1) GetItem: /courses?courseId=CS101  (假设单主键 courseId)
    course_id = q.get("courseId")
    if course_id:
        res = table.get_item(Key={"courseId": course_id})
        item = res.get("Item")
        if not item:
            return _resp(404, {"message": "Not found", "courseId": course_id})
        return _resp(200, {"item": item})

    # 2) Query: /courses?pk=DEPT#CS&skPrefix=COURSE#  (假设复合主键 pk+sk)
    pk = q.get("pk")
    sk_prefix = q.get("skPrefix")
    if pk and sk_prefix:
        res = table.query(
            KeyConditionExpression=Key("pk").eq(pk) & Key("sk").begins_with(sk_prefix),
            Limit=50,
        )
        return _resp(200, {"count": res.get("Count", 0), "items": res.get("Items", [])})

    # 3) Scan: /courses  (⚠️不推荐)
    res = table.scan(Limit=50)
    return _resp(200, {"count": res.get("Count", 0), "items": res.get("Items", [])})
```

---

### 2) `template.yaml`（对应 CodeUri / Handler / API 路由）

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: getCourses API

Globals:
  Function:
    Runtime: python3.12
    Timeout: 10
    MemorySize: 128

Resources:
  CoursesApi:
    Type: AWS::Serverless::Api
    Properties:
      StageName: dev
      Cors:
        AllowMethods: "'GET,OPTIONS'"
        AllowHeaders: "'Content-Type,Authorization'"
        AllowOrigin: "'*'"

  GetCoursesFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: get-courses
      CodeUri: src/get_courses/           # ✅目录指到函数文件夹
      Handler: app.lambda_handler          # ✅对应 app.py 的 lambda_handler
      Environment:
        Variables:
          COURSES_TABLE: Courses           # ✅表名
      Policies:
        - AWSLambdaBasicExecutionRole      # ✅CloudWatch Logs
        # ✅最小 DynamoDB 读权限（推荐你用最小权限，而不是 DynamoDBFullAccess）
        - Statement:
            - Effect: Allow
              Action:
                - dynamodb:GetItem
                - dynamodb:Query
                - dynamodb:Scan
              Resource:
                - !Sub arn:aws:dynamodb:${AWS::Region}:${AWS::AccountId}:table/Courses
      Events:
        GetCoursesApi:
          Type: Api
          Properties:
            RestApiId: !Ref CoursesApi
            Path: /courses
            Method: GET

Outputs:
  CoursesApiUrl:
    Value: !Sub "https://${CoursesApi}.execute-api.${AWS::Region}.amazonaws.com/dev/courses"
    Description: GET /courses endpoint
```

---

### 3) 本地跑（你能立刻用）

`events/getCourses.json`

```json
{
  "queryStringParameters": {
    "courseId": "CS101"
  }
}
```

命令：

```bash
sam build
sam local invoke GetCoursesFunction -e events/getCourses.json
```

如果你想走 HTTP 方式（更像真实 API）：

```bash
sam local start-api
# 然后：
curl "http://127.0.0.1:3000/courses?courseId=CS101"
```

---

## 🧪 3 道 “CodeUri / Handler / 目录结构” 易错题（高频踩坑）

### 题 1：`CodeUri: src/`，`Handler: get_courses.app.lambda_handler`，但启动报 “Unable to import module”

**错因**：`CodeUri` 指向 `src/` 时，Python import 路径要能找到 `get_courses/app.py` 且 `get_courses` 是 package（需要 `__init__.py`），否则导入失败。
**正确两种修法**：

* **推荐**：`CodeUri: src/get_courses/` + `Handler: app.lambda_handler`
* 或者：保留 `CodeUri: src/`，但加 `src/get_courses/__init__.py`，并确保 Handler 路径正确

---

### 题 2：我把 handler 文件叫 `main.py`，但模板写 `Handler: app.lambda_handler`，会发生什么？

**结果**：运行时会找 `app.py`，找不到就直接 import fail。
**修法**：要么把文件名改回 `app.py`，要么把 Handler 改成 `main.lambda_handler`。

---

### 题 3：代码放在 `src/get_courses/app.py`，但 `CodeUri: src/get_courses/app.py`（指向文件）为什么会失败？

**错因**：`CodeUri` 期望是**目录或 zip 包**（SAM 会把目录打包），指向单个 `.py` 文件会导致打包/构建异常或运行找不到依赖结构。
**修法**：`CodeUri: src/get_courses/`

---

如果你把 **Courses 表的 key 结构**（单主键 `courseId` 还是 `pk+sk`）贴一下，我可以把 Lambda 改成**完全匹配你表的 Query 模式**（含分页 `LastEvaluatedKey` + `ExclusiveStartKey`，以及最小 IAM 权限到具体 ARN）。
