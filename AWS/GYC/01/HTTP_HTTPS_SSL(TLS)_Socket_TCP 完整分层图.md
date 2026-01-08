## 1) “HTTP / HTTPS / SSL(TLS) / Socket / TCP”完整分层图（ASCII）

```
┌──────────────────────────────────────────────┐
│ Application Layer (应用层)                   │
│  HTTP  (网页协议：请求/响应)                 │
│  HTTPS = HTTP + TLS(SSL)                     │
└──────────────────────────────────────────────┘
                 │
┌──────────────────────────────────────────────┐
│ Security Layer (加密层，插在应用和传输之间)  │
│  TLS/SSL：握手 + 证书 + 密钥交换 + 加密传输  │
└──────────────────────────────────────────────┘
                 │
┌──────────────────────────────────────────────┐
│ Transport Layer (传输层)                     │
│  TCP：可靠、有序、面向连接                   │
│  UDP：不保证到达、无连接、低延迟             │
└──────────────────────────────────────────────┘
                 │
┌──────────────────────────────────────────────┐
│ Network Layer (网络层)                       │
│  IP：负责寻址与路由                           │
└──────────────────────────────────────────────┘
                 │
┌──────────────────────────────────────────────┐
│ Link/Physical (链路/物理层)                   │
│  Ethernet / Wi-Fi / 光纤 / 电信号             │
└──────────────────────────────────────────────┘

Socket（套接字）不是“某一层协议”
→ 它是应用层用来“操作 TCP/UDP”的编程接口（API/抽象）
→ 你用 socket 来 connect/send/recv，本质是在用 TCP/UDP
```

---

## 2) 最小 socket 示例（Java + Python）

### Java：TCP Echo（Server + Client）

```java
// EchoServer.java
import java.io.*;
import java.net.*;

public class EchoServer {
    public static void main(String[] args) throws Exception {
        int port = 9090;
        ServerSocket server = new ServerSocket(port); // 1) 监听端口
        System.out.println("Server listening on " + port);

        Socket client = server.accept();              // 2) 接受连接（TCP三次握手完成后返回）
        System.out.println("Client connected");

        BufferedReader in = new BufferedReader(new InputStreamReader(client.getInputStream()));
        BufferedWriter out = new BufferedWriter(new OutputStreamWriter(client.getOutputStream()));

        String line = in.readLine();                  // 3) 读取一行
        out.write("echo: " + line + "\n");            // 4) 回写
        out.flush();

        client.close();
        server.close();
    }
}
```

```java
// EchoClient.java
import java.io.*;
import java.net.*;

public class EchoClient {
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("127.0.0.1", 9090); // 1) 连接服务器
        BufferedWriter out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
        BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

        out.write("hello\n");                          // 2) 发消息
        out.flush();

        System.out.println(in.readLine());             // 3) 收回包
        socket.close();
    }
}
```

运行：

1. 先 `java EchoServer`
2. 再 `java EchoClient`

---

### Python：TCP Echo（Server + Client）

```python
# echo_server.py
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("0.0.0.0", 9090))
server.listen(1)

conn, addr = server.accept()
data = conn.recv(1024)
conn.sendall(b"echo: " + data)

conn.close()
server.close()
```

```python
# echo_client.py
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("127.0.0.1", 9090))
s.sendall(b"hello\n")
print(s.recv(1024).decode())
s.close()
```

---

## 3) 5 道 socket / SSL / HTTPS 判断题（带答案）

1. **HTTPS = HTTP + TCP**
   ❌ 错：HTTPS = **HTTP + TLS(SSL) + TCP**

2. **TLS(SSL) 的握手阶段会用到非对称加密**
   ✅ 对：用于**身份认证 + 密钥交换**

3. **Socket 是一种网络协议**
   ❌ 错：Socket 是**编程接口/抽象**，让应用使用 TCP/UDP

4. **TLS 加密后，TCP 依然负责可靠传输**
   ✅ 对：TLS 解决“保密/完整性/认证”，TCP 解决“可靠/有序”

5. **如果 ALB 终止 TLS，那么 ALB→EC2 一定是 HTTPS**
   ❌ 错：通常是 **HTTP**（除非你配置 re-encrypt / end-to-end TLS）

---

## 4) TCP vs UDP（考试版对比）

| 点    | TCP             | UDP            |
| ---- | --------------- | -------------- |
| 连接   | 面向连接（三次握手）      | 无连接            |
| 可靠性  | ✅ 保证到达、重传、顺序    | ❌ 不保证          |
| 延迟   | 较高              | ✅ 更低           |
| 适合场景 | 网页/支付/API/文件传输  | 直播/语音/游戏/监控指标  |
| 常见协议 | HTTP/HTTPS, SSH | DNS, VoIP, RTP |

📌 秒选口诀：

* **要可靠 = TCP**
* **要低延迟/可丢包 = UDP**

---

## 5) 你想继续：底层网络 还是 AWS 架构？

给你两个清晰路线（你直接回 A 或 B）：

**A) 底层网络（更硬核）**

* 三次握手/四次挥手
* TIME_WAIT / 半连接队列
* TLS Handshake 细化（ClientHello/证书/密钥交换）
* 常见抓包题怎么答

**B) AWS 架构（更考试）**

* ALB/NLB + TLS 终止/透传
* CloudFront + HTTPS + ACM
* 私网/公网、NAT、SG/NACL 与端口
* “端到端加密”考试陷阱题

你选 **A 底层网络** 还是 **B AWS 架构**？
