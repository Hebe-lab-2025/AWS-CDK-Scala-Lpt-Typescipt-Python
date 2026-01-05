
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

So the statement “YAML is for unstructured data” is incorrect — YAML is for **structured, hierarchical configuration data**.

