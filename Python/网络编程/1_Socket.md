## Socket

Socket(简称 套接字) 是进程间通信的一种方式，能实现不同主机间的进程间通信，我们网络上各种各样的服务大多都是基于 Socket 来完成通信的。



```python
# 语法
socket.socket(AddressFamily, Type)
```

函数 socket.socket 创建一个 socket，该函数带有两个参数：

- Address Family：可以选择 AF_INET（用于 Internet 进程间通信） 或者 AF_UNIX（用于同一台机器进程间通信）,实际工作中常用AF_INET
- Type：套接字类型，可以是 SOCK_STREAM（流式套接字，主要用于 TCP 协议）或者 SOCK_DGRAM（数据报套接字，主要用于 UDP 协议）



```python
# 创建一个tcp socket
from socket import *
s=socket(AF_INET,SOCK_STREAM)
pass
s.close()
```



```python
# 创建一个udp socket
from socket import *
s=socket(AF_INET,SOCK_DGRAM)
pass
s.close()
```







