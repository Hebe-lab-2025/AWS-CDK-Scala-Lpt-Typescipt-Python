这是 **CloudWatch Logs Insights** 的完整查询语句，按你给的要求写好可直接用 👇

```sql
fields @timestamp, @message
| sort @timestamp desc
| limit 10000
```

### 每一行在干什么（速记）

* `fields @timestamp, @message`
  👉 只显示时间戳和日志内容
* `sort @timestamp desc`
  👉 按时间 **从新到旧**
* `limit 10000`
  👉 最多返回 10000 条日志

---

### 🔍 如果你想**只看某个 Request / Trace ID**（比如你给的这个）

```sql
fields @timestamp, @message
| filter @message like /1d574722-912a-40cf-8305-5cc15d721c8f/
| sort @timestamp desc
| limit 10000
```

---

### 🧠 面试 / 实战一句话

> CloudWatch Logs Insights 通过 fields + filter + sort + limit 组合，实现高效日志检索与问题回溯。

如果你愿意，我可以帮你：

* 🔥 写 **Lambda / API Gateway 常用日志模板**
* 🔍 拆 **@requestId / @logStream / @logGroup**
* 🧪 出 5 道 Logs Insights 排错题



