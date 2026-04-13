---
title: Java 13 - 网络编程
date: 2017-01-05
tags: [Java]
categories:
  - 编程语言
  - Java
---

## Socket 基础

客户端和服务器通过 Socket 建立连接，基于 TCP。

### 客户端

```java
import java.net.*;
import java.io.*;

Socket socket = new Socket("127.0.0.1", 8080);  // 连接服务器

// 获取输入输出流
PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
BufferedReader in = new BufferedReader(
    new InputStreamReader(socket.getInputStream()));

out.println("Hello Server");          // 发送数据
String response = in.readLine();      // 接收数据
System.out.println("Server: " + response);

socket.close();
```

### 服务器端

```java
ServerSocket server = new ServerSocket(8080);  // 监听端口
System.out.println("Server started...");

Socket client = server.accept();  // 阻塞，等待客户端连接
System.out.println("Client connected: " + client.getInetAddress());

BufferedReader in = new BufferedReader(
    new InputStreamReader(client.getInputStream()));
PrintWriter out = new PrintWriter(client.getOutputStream(), true);

String msg = in.readLine();
System.out.println("Client: " + msg);
out.println("Hello Client");

client.close();
server.close();
```

## 多线程服务器

```java
public class MultiThreadServer {
    public static void main(String[] args) throws IOException {
        ServerSocket server = new ServerSocket(8080);

        while (true) {
            Socket client = server.accept();
            // 每个客户端开一个线程处理
            new Thread(new ClientHandler(client)).start();
        }
    }
}

class ClientHandler implements Runnable {
    private Socket client;

    ClientHandler(Socket client) { this.client = client; }

    @Override
    public void run() {
        try {
            BufferedReader in = new BufferedReader(
                new InputStreamReader(client.getInputStream()));
            PrintWriter out = new PrintWriter(client.getOutputStream(), true);

            String line;
            while ((line = in.readLine()) != null) {
                System.out.println("Received: " + line);
                out.println("Echo: " + line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try { client.close(); } catch (IOException e) {}
        }
    }
}
```

## URL 与 HTTP

```java
import java.net.*;
import java.io.*;

URL url = new URL("http://example.com");
BufferedReader br = new BufferedReader(
    new InputStreamReader(url.openStream()));

String line;
while ((line = br.readLine()) != null)
    System.out.println(line);
br.close();
```

## 常见坑

- `server.accept()` 是阻塞调用，没有客户端连接时会一直等待
- 端口 0~1023 是知名端口，需要管理员权限；程序一般用 1024~65535
- Socket 通信是双向的，客户端/服务器都有 InputStream 和 OutputStream
- 一定要关闭 Socket，否则端口会被占用
