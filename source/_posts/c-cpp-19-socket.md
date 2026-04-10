---
title: C/C++学习总结（十九）——Socket网络编程
date: 2016-11-04
categories:
  - 全栈开发
  - 后端
tags:
  - C
  - 网络
---

## TCP Socket 流程

**服务端：**
```
socket() → bind() → listen() → accept() → recv/send → close()
```

**客户端：**
```
socket() → connect() → send/recv → close()
```

## 服务端代码框架

```c
#include <sys/socket.h>
#include <netinet/in.h>

int server_fd = socket(AF_INET, SOCK_STREAM, 0);

struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(8080);
addr.sin_addr.s_addr = INADDR_ANY;

bind(server_fd, (struct sockaddr*)&addr, sizeof(addr));
listen(server_fd, 5);

int client_fd = accept(server_fd, NULL, NULL);

char buf[1024];
recv(client_fd, buf, sizeof(buf), 0);
send(client_fd, "ok", 2, 0);

close(client_fd);
close(server_fd);
```

## 客户端代码框架

```c
int sock = socket(AF_INET, SOCK_STREAM, 0);

struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(8080);
inet_pton(AF_INET, "127.0.0.1", &addr.sin_addr);

connect(sock, (struct sockaddr*)&addr, sizeof(addr));
send(sock, "hello", 5, 0);

close(sock);
```

## 字节序转换

网络传输用大端字节序，主机不一定，需要转换：

```c
htons()  // host to network, short（端口用这个）
htonl()  // host to network, long
ntohs()  // 反向
ntohl()  // 反向
```

---

**问题：** bind 失败，报错 "Address already in use"
**解决：** 设置 SO_REUSEADDR
```c
int opt = 1;
setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));
```
在 bind 之前调用。

---

**问题：** connect 失败，报错 "Connection refused"
**解决：** 确认服务端已启动且端口正确，或检查防火墙设置。

---

**问题：** recv 返回0
**解决：** 返回0表示对端已关闭连接，需处理断开逻辑，不能继续读。
```c
int n = recv(fd, buf, sizeof(buf), 0);
if (n == 0) { close(fd); }
```
