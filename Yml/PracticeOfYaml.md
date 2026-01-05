**Answer: C. !!dict**

### 🔹 English explanation

`!!seq`, `!!omap`, and `!!set` are valid YAML tags.
`!!dict` is **not** a standard YAML tag.

* `!!seq` → sequence (list)
* `!!omap` → ordered map
* `!!set` → set
* `!!dict` → ❌ not defined in YAML spec

---

### 🧾 English examples of valid tags

**Sequence**

```
numbers: !!seq
  - 1
  - 2
  - 3
```

**Ordered map**

```
grades: !!omap
  - Alice: 90
  - Bob: 88
```

**Set**

```
letters: !!set
  a: null
  b: null
```

---

### 🔹 中文解释

`!!seq`、`!!omap`、`!!set` 都是 **合法 YAML 标签**，
而 `!!dict` **不是 YAML 标准标签**，因此是错误选项。

* `!!seq` → 序列（列表）
* `!!omap` → 有序映射
* `!!set` → 集合
* `!!dict` → ❌ 非标准标签

---

需要下一题吗？


**Answer: A. True**

### 🔹 English explanation

In YAML, **tags are used to explicitly specify data types**.
Normally YAML auto-detects types (number, boolean, string), but tags let you **override or declare types manually**, or define **custom application-specific types**.

### 🧾 English examples

**Explicit integer**

```
age: !!int "25"
```

**Explicit string (even if looks like number)**

```
zip: !!str 00123
```

**Explicit boolean**

```
flag: !!bool "true"
```

**Custom tag**

```
person: !User
  name: Alice
  age: 16
```

---

### 🔹 中文解释

在 YAML 中，**tag 用来显式指定数据类型**。
平时 YAML 会自动识别类型，但通过 tag 可以：

* 强制指定类型
* 防止被误解析
* 定义自定义类型（给程序使用）

### 🧾 中文示例

**显式整数**

```
age: !!int "25"
```

**显式字符串（防止被当成数字）**

```
zip: !!str 00123
```

**自定义类型**

```
user: !Student
  name: Alice
  grade: 9
```

---

Ready for the next question 😊


**Answer: A. True**

### 🔹 English explanation

In YAML, **any line that begins with `#` is a comment**.
Everything after `#` on that line is ignored by the YAML parser and not treated as data. Comments are used to explain configuration, disable lines, or leave notes for readers.

---

### 🧾 English examples

**Example 1 — full-line comment**

```
# This is a YAML comment
name: Alice
```

**Example 2 — comment after a value**

```
port: 8080  # application port number
```

**Example 3 — multiple comments**

```
# App configuration
env: prod
# Database config below
db: mysql
```

---

### 🔹 中文解释

在 YAML 中，**任何以 `#` 开头的行都是注释**。
同一行中从 `#` 开始后的内容都会被忽略，不参与解析。
注释通常用于说明配置、写备注或临时停用某些内容。

---

### 🧾 中文示例

**示例 1 — 整行注释**

```
# 这是注释
name: Alice
```

**示例 2 — 行尾注释**

```
port: 8080  # 应用端口号
```

**示例 3 — 多行注释**

```
# 应用配置
env: prod
# 数据库配置
db: mysql
```

---

👉 下一题继续吗？

**Answer: B. `<Key>: <Value>`**

### 🔹 English explanation

In YAML, a **mapping** means a key–value pair.
It is written using a **colon** between the key and the value.
There should be a space after the colon to keep it readable and valid.

**Example**

```
name: Alice
age: 16
city: Seattle
```

The other options are incorrect because YAML does **not** use `-`, `~`, or `?` to define normal mappings.

---

### 🔹 中文解释

在 YAML 中，**mapping 表示键值对**。
正确写法是在 **键和值之间使用冒号**，并在冒号后空一格。

**示例**

```
name: Alice
age: 16
city: Seattle
```

其他写法（`-`、`~`、`?`）都不是 YAML 中普通键值对的语法。

---

如果你愿意，我可以继续陪你做下一题 😊

**Answer: B. `<Key>: <Value>`**

This is the correct syntax for a mapping (key–value pair) in YAML.

### ✅ Correct example

```yaml
name: Alice
age: 15
city: Seattle
```

### ❌ Incorrect examples (do NOT use)

```
name - Alice
name ~ Alice
name ? Alice
```

### 💡 Quick tip

* The **colon ":"** separates key and value.
* A space after the colon is recommended.

**Answer: B. False**

Strings in YAML **do not always need quotes**.
Quotes are only required when:

* the text could be misinterpreted (e.g., `yes`, `no`, `on`, `off`, numbers)
* the string contains special characters (`:`, `#`, `{}`, `[]`)
* you want to preserve leading/trailing spaces
* you want multi-line or escape processing

---

### ✅ Example 1 — no quotes needed

```yaml
city: Seattle
language: English
```

---

### ✅ Example 2 — quotes recommended

(because the value looks like boolean or number)

```yaml
flag: "true"
version: "01"
```

Without quotes, YAML might convert them to boolean or integer.

---

### ✅ Example 3 — quotes required

(colon inside the string)

```yaml
title: "DevOps: YAML Basics"
```

---

### ❌ Incorrect example (without quotes, YAML breaks)

```yaml
message: Hello: world   # colon confuses parser
```

---

If you’d like, next we can do:

* more quizzes
* YAML vs JSON comparison
* Spring Boot `application.yml` real example

Sure — here are a few **simple YAML nesting examples**.

### 🔹 Example 1 — mapping contains a sequence

```yaml
student:
  name: Alice
  grades:
    - Math
    - Physics
    - English
```

* `student` is a mapping
* `grades` is a list inside it ✔️

---

### 🔹 Example 2 — sequence contains mappings

```yaml
employees:
  - name: Tom
    role: Developer
  - name: Lisa
    role: Tester
```

* `employees` is a list
* each item is a mapping ✔️

---

### 🔹 Example 3 — mixed deep nesting

```yaml
school:
  classes:
    - name: ClassA
      students:
        - name: Anna
          age: 12
        - name: Ben
          age: 13
```

Here you have:

* mapping → `school`
* mapping → `classes`
* sequence → list of classes
* mapping inside list → each class
* another sequence → students list
* mapping inside it → student info

---

If you want, I can also show:

* ❇️ YAML vs JSON same structure comparison
* ⚙️ Spring Boot application.yml real example
* 🐳 Docker Compose nested example


**Answer: A. True**

Sequences (lists) and mappings (key–value pairs) in YAML **can be nested inside one another** 
to represent hierarchical data structures.


**Answer: A. True**

Yes — both `null` and `~` have a **special meaning in YAML**: 
they both represent a **null value** (no value).

**Answer: B. False**

YAML **is case sensitive**.
Keys, values like `true/True/TRUE`, anchors, and aliases are all treated differently based on case. 
For example, `name` and `Name` are considered two different keys.

**Answer: A. True**

YAML is commonly used to store configuration settings because it is human-readable 
and supports hierarchical key–value structures. 
That’s why it’s widely used in tools like Spring Boot (`application.yml`), 
Docker Compose, Kubernetes manifests, Ansible, and many CI/CD systems.

The correct answer is **B. False**.

**Why:**
YAML is actually used to represent **structured data** (hierarchical key–value data, lists, maps). It is human-readable, but the data itself is **well-structured**, not unstructured.

**Key points to remember**

* YAML = “YAML Ain’t Markup Language”
* Designed for **configuration files** and **data serialization**
* Used in **Spring Boot, Docker Compose, Kubernetes manifests, Ansible, CI/CD configs**
* Supports clear structure: indentation, lists, dictionaries, nesting

**Example structured YAML**

```yaml
server:
  port: 8080
database:
  host: localhost
  users:
    - admin
    - guest
```

So the statement “YAML is for unstructured data” is incorrect — 
YAML is for **structured, hierarchical configuration data**.





