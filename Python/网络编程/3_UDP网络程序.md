## UDP网络程序

创建一个基于udp的网络程序流程具体步骤如下：

- 创建客户端套接字
- 发送/接收数据
- 关闭套接字

![20200821205203823](D:\Notes\Python\网络编程\image\20200821205203823.png)



### 1 发送数据

```python
from socket import *
udp_socket=socket(AF_INET,SOCK_DGRAM)   # 创建udp socket
dest_addr=("192.168.101.82",8080)       # 接收方的地址和端口
send_data=input("请输入要发送的数据:")
udp_socket.sendto(send_data.encode('utf-8'),dest_addr)  # 发送数据到udp服务端
udp_socket.close()      # 关闭 socket
```

udp服务端：在windows中运行“网络调试助手”

![20200821210434306](D:\Notes\Python\网络编程\image\20200821210434306.png)

输入：linyi，服务端接收信息。



### 2 发送、接收数据

```python
from socket import *
udp_socket=socket(AF_INET,SOCK_DGRAM)   # 创建udp socket
dest_addr=("192.168.101.82",8080)       # 接收方的地址和端口
send_data=input("请输入要发送的数据:")
udp_socket.sendto(send_data.encode('gbk'),dest_addr)  # 发送数据到udp服务端
recv_data=udp_socket.recvfrom(1024)     # 接收数据，1024表示本次接收的最大字节数
print(recv_data[0].decode('gbk'))   	# recv_data是一个元组，第1个元素是对方发送的数据
print(recv_data[1])                 	# 第2个元素是对方的ip和端口
udp_socket.close()      				# 关闭 socket
```



### 3 UDP绑定端口

```python
from socket import *
udp_socket=socket(AF_INET,SOCK_DGRAM)   # 创建udp socket
local_addr=('',7788)
udp_socket.bind(local_addr)     # 绑定端口
recv_data=udp_socket.recvfrom(1024)
print(recv_data[0].decode('gbk'))
udp_socket.close()
```



### 4 UDP聊天器

```python
"""udp聊天器"""
from socket import *
 
def send_msg(udp_socket):
    """获取键盘数据，并将其发送给对方"""
    dest_ip=input("\n请输入对方的ip地址:")
    dest_port=int(input("\n请输入对方的port:"))
    msg=input("\n请输入要发送的数据:")
    udp_socket.sendto(msg.encode("gbk"),(dest_ip,dest_port))
 
def recv_msg(udp_socker):
    """接收数据并显示"""
    recv_msg=udp_socker.recvfrom(1024)
    recv_ip=recv_msg[1]
    recv_msg=recv_msg[0].decode("gbk")
    print(">>{0}:{1}".format(str(recv_ip),recv_msg))
 
def main():
    """功能选择"""
    udp_socket=socket(AF_INET,SOCK_DGRAM)
    udp_socket.bind(('',9999))
    while True:
        print("="*30)
        print("1:发送消息")
        print("2:接收消息")
        print("3:接收消息")
        print("="*30)
        op_num=input("请输入要操作的功能序号:")
        if op_num=="1":
            send_msg(udp_socket)
        elif op_num=="2":
            recv_msg(udp_socket)
        elif op_num=="3":
            break
        else:
            print("输入有误，请重新输入...")
 
if __name__=='__main__':
    main()
```





