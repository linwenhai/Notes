## TCP网络程序

TCP协议：传输控制协议（英语：Transmission Control Protocol，缩写为 TCP）是一种面向连接的、可靠的、基于字节流的传输层通信协议。

TCP通信需要经过创建连接、数据传送、终止连接三个步骤。

![20200821214902472](D:\Notes\Python\网络编程\image\20200821214902472.png)



### 1 TCP客户端

```python
# TCP客户端
from socket import *
tcp_client_socket=socket(AF_INET,SOCK_STREAM)   # 创建TCP socket
server_id=input("请输入服务器ip:")
server_port=int(input("请输入服务器port:"))
tcp_client_socket.connect((server_id,server_port))  # 链接服务器
send_data=input("请输入要发送的数据：")
tcp_client_socket.send(send_data.encode("gbk"))
recv_data=tcp_client_socket.recv(1024)
print("接收到的数据为:",recv_data.decode("gbk"))
tcp_client_socket.close()
```



### 2 TCP服务端

```python
# TCP服务端
from socket import *
tcp_server_socket=socket(AF_INET,SOCK_STREAM)   # 创建 tcp socket
addr=('',9999)
tcp_server_socket.bind(addr)    # 绑定端口
tcp_server_socket.listen(128)   # 默认的属性是主动的，使用listen将其变为被动的
client_socket,clientaddr=tcp_server_socket.accept()
recv_data=client_socket.recv(1024)
print("接收到的数据为:",recv_data.decode("gbk"))
client_socket.send("谢谢".encode("gbk"))
client_socket.close()
```



### 3 文件下载器

```python
# 文件下载器服务端
from socket import *
import sys
def get_file_content(file_name):
    """获取文件的内容"""
    try:
        with open(file_name,"rb") as f:
            content=f.read()
        return content
    except:
        print("没有下载的文件:",file_name)
 
def main():
    tcp_server_socket=socket(AF_INET,SOCK_STREAM)
    addr=('',9999)
    tcp_server_socket.bind(addr)
    tcp_server_socket.listen(128)
    while True:
        client_socket,clientaddr=tcp_server_socket.accept()
        recv_data=client_socket.recv(1024)
        file_name=recv_data.decode("gbk")
        print("对方请求下载的文件名为:",file_name)
        file_content=get_file_content(file_name)
        if file_content:
            client_socket.send(file_content)
        client_socket.close()
    tcp_server_socket.close()
 
if __name__=='__main__':
    main()
```

