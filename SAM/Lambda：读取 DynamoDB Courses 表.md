## 🔍 Lambda：读取 DynamoDB `Courses` 表（可 Query / Get / Scan）

下面给你一份**实战可用**的 Python Lambda（API Gateway 事件也能用）：

* `GET /courses?courseId=CS101` → **GetItem**
* `GET /courses?pk=DEPT#CS&skPrefix=COURSE#` → **Query + begins_with**
* `GET /courses` → **Scan（不推荐，只有 demo 用）**

> 你只要把环境变量 `COURSES_TABLE` 设成 `Courses` 即可。

```python
import os
import json
import boto3
from boto3.dynamodb.conditions import Key

dynamodb = boto3.resource("dynamodb")
TABLE_NAME = os.environ.get("COURSES_TABLE", "Courses")
table = dynamodb.Table(TABLE_NAME)

def _resp(status_code: int, body_obj):
    return {
        "statusCode": status_code,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps(body_obj, default=str),
    }

def lambda_handler(event, context):
    # 兼容 API Gateway（HTTP API / REST API）的 queryStringParameters
    q = (event or {}).get("queryStringParameters") or {}

    # 1) GetItem: /courses?courseId=CS101
    # 假设你的 Courses 主键是 courseId（Partition Key）
    course_id = q.get("courseId")
    if course_id:
        try:
            res = table.get_item(Key={"courseId": course_id})
            item = res.get("Item")
            if not item:
                return _resp(404, {"message": "Course not found", "courseId": course_id})
            return _resp(200, {"item": item})
        except Exception as e:
            return _resp(500, {"message": "GetItem failed", "error": str(e)})

    # 2) Query: /courses?pk=DEPT#CS&skPrefix=COURSE#
    # 假设你的表是复合主键：pk + sk（推荐建模方式）
    pk = q.get("pk")
    sk_prefix = q.get("skPrefix")
    if pk and sk_prefix:
        try:
            res = table.query(
                KeyConditionExpression=Key("pk").eq(pk) & Key("sk").begins_with(sk_prefix),
                Limit=50,
            )
            return _resp(200, {"count": res.get("Count", 0), "items": res.get("Items", [])})
        except Exception as e:
            return _resp(500, {"message": "Query failed", "error": str(e)})

    # 3) Scan: /courses  （⚠️不推荐，读全表很贵）
    try:
        res = table.scan(Limit=50)
        return _resp(200, {"count": res.get("Count", 0), "items": res.get("Items", [])})
    except Exception as e:
        return _resp(500, {"message": "Scan failed", "error": str(e)})
```

**最常见的 IAM 权限（Lambda role）**：

* `dynamodb:GetItem`
* `dynamodb:Query`
* `dynamodb:Scan`（如果你真要 scan）
* `dynamodb:DescribeTable`（可选）

---

## 🧪 3 道 DynamoDB BatchWrite（BatchWriteItem）易错题（真面试/真踩坑）

### 题 1：你用 BatchWrite 一次写 100 条，返回 200 但数据少了几条，为什么？

**答案要点：**

* BatchWrite **可能返回 `UnprocessedItems`**（吞吐/限流/内部重试失败）
* 你必须对 `UnprocessedItems` 做 **指数退避重试**，直到清空或达到上限
* “200” 只代表请求成功接收，不代表每条都写入成功

---

### 题 2：BatchWrite 能 “更新” 某个 Item 的部分字段吗？

**答案要点：**

* **不行**。BatchWrite 只支持 **PutItem / DeleteItem**
* PutItem 是 **全量覆盖**（同主键会覆盖原 item，不存在 partial update）
* 要做部分字段更新：用 `UpdateItem`（但它不在 BatchWrite 里）

---

### 题 3：BatchWrite 为什么不能保证“要么都成功，要么都失败”？

**答案要点：**

* BatchWrite **不支持事务**（没有原子性）
* 每条 Put/Delete 独立成功或失败
* 要原子性：用 `TransactWriteItems`（有 25 条限制、也更贵）

> Bonus 易错点：BatchWrite **单次最多 25 个请求**（Put/Delete 混合也算）。

---

## 🧠 CLI JSON → SDK 对照（Java / JS）

下面用同一件事对照：**BatchWrite 写入 2 条 Course**（PutRequest）

### 1) CLI（batch-write-item）JSON

`batch.json`

```json
{
  "Courses": [
    {
      "PutRequest": {
        "Item": {
          "courseId": { "S": "CS101" },
          "title":    { "S": "Intro to CS" },
          "credits":  { "N": "3" }
        }
      }
    },
    {
      "PutRequest": {
        "Item": {
          "courseId": { "S": "CS102" },
          "title":    { "S": "Data Structures" },
          "credits":  { "N": "4" }
        }
      }
    }
  ]
}
```

CLI 调用：

```bash
aws dynamodb batch-write-item --request-items file://batch.json
```

---

### 2) Java（AWS SDK v2）等价写法

```java
import software.amazon.awssdk.services.dynamodb.DynamoDbClient;
import software.amazon.awssdk.services.dynamodb.model.*;

import java.util.*;

public class BatchWriteCourses {
    public static void main(String[] args) {
        DynamoDbClient ddb = DynamoDbClient.create();
        String table = "Courses";

        // Item1
        Map<String, AttributeValue> item1 = new HashMap<>();
        item1.put("courseId", AttributeValue.builder().s("CS101").build());
        item1.put("title", AttributeValue.builder().s("Intro to CS").build());
        item1.put("credits", AttributeValue.builder().n("3").build());

        // Item2
        Map<String, AttributeValue> item2 = new HashMap<>();
        item2.put("courseId", AttributeValue.builder().s("CS102").build());
        item2.put("title", AttributeValue.builder().s("Data Structures").build());
        item2.put("credits", AttributeValue.builder().n("4").build());

        WriteRequest wr1 = WriteRequest.builder()
                .putRequest(PutRequest.builder().item(item1).build())
                .build();

        WriteRequest wr2 = WriteRequest.builder()
                .putRequest(PutRequest.builder().item(item2).build())
                .build();

        Map<String, List<WriteRequest>> requestItems = new HashMap<>();
        requestItems.put(table, Arrays.asList(wr1, wr2));

        BatchWriteItemRequest req = BatchWriteItemRequest.builder()
                .requestItems(requestItems)
                .build();

        BatchWriteItemResponse resp = ddb.batchWriteItem(req);

        // ✅关键：检查 UnprocessedItems（真实项目要循环重试 + backoff）
        if (resp.hasUnprocessedItems() && !resp.unprocessedItems().isEmpty()) {
            System.out.println("UnprocessedItems found: " + resp.unprocessedItems());
        } else {
            System.out.println("BatchWrite succeeded");
        }

        ddb.close();
    }
}
```

---

### 3) JavaScript（AWS SDK v3）等价写法

> 这里用 **DynamoDBClient（低层）**，结构和 CLI 最像。

```js
import { DynamoDBClient, BatchWriteItemCommand } from "@aws-sdk/client-dynamodb";

const client = new DynamoDBClient({});
const TableName = "Courses";

export async function run() {
  const cmd = new BatchWriteItemCommand({
    RequestItems: {
      [TableName]: [
        {
          PutRequest: {
            Item: {
              courseId: { S: "CS101" },
              title: { S: "Intro to CS" },
              credits: { N: "3" },
            },
          },
        },
        {
          PutRequest: {
            Item: {
              courseId: { S: "CS102" },
              title: { S: "Data Structures" },
              credits: { N: "4" },
            },
          },
        },
      ],
    },
  });

  const resp = await client.send(cmd);

  // ✅关键：UnprocessedItems 要重试
  if (resp.UnprocessedItems && Object.keys(resp.UnprocessedItems).length > 0) {
    console.log("UnprocessedItems found:", resp.UnprocessedItems);
  } else {
    console.log("BatchWrite succeeded");
  }
}

run().catch(console.error);
```

---

### （可选）你更常用的 SDK“高层写法”

如果你想用更舒服的 JSON（不用 `{S:...}`），我也可以给你 **DynamoDBDocumentClient（JS）** / **Enhanced Client（Java）** 版本。

---

如果你告诉我 `Courses` 表的 key 结构（是 `courseId` 单主键，还是 `pk+sk` 复合主键？），我可以把上面的 Lambda 直接改成**完全贴合你表结构**，并补一版：

* `sam local invoke` 的 `event.json`
* `aws dynamodb query/get-item` 的 CLI 命令（和 SDK 对照一一对应）
