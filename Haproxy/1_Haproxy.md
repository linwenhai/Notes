### Haproxy



### 1 查看OS内核

```shell
uname -r
```

```
3.10.0-957.el7.x86_64
```



### 2 下载lua及haproxy源码包

haproxy-1.9.7.tar.gz

lua-5.3.5.tar.gz

```shell
cd /opt
curl https://www.lua.org/ftp/lua-5.3.5.tar.gz > lua-5.3.5.tar.gz
curl http://www.haproxy.org/download/1.9/src/haproxy-1.9.7.tar.gz > haproxy-1.9.7.tar.gz
```



### 3 安装lua

```shell
tar -zxvf lua-5.3.5.tar.gz
cd lua-5.3.5
make INSTALL_TOP=/usr/local/lua-5.3.5 linux install
```



### 4 安装haproxy

```shell
tar -zxvf haproxy-1.9.7.tar.gz
cd haproxy-1.9.7
make TARGET=linux3100 PREFIX=/opt/haproxy
make install PREFIX=/opt/haproxy
cp -r /opt/haproxy-1.9.7/examples/errorfiles/ /opt/haproxy/
mkdir -p /opt/haproxy/conf
```

```shell
# 查看版本号
sbin/haproxy -v
```





### 5 haproxy.cfg

```shell
vi /opt/haproxy/conf/haproxy.cfg
```



```shell
global
    daemon
    maxconn 256
    user        root
    group       root
    chroot      /opt/haproxy
    log 127.0.0.1 local0 info
    log 127.0.0.1 local1 warning


defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms
    errorfile 400 /opt/haproxy/errorfiles/400.http
    errorfile 403 /opt/haproxy/errorfiles/403.http
    errorfile 408 /opt/haproxy/errorfiles/408.http
    errorfile 500 /opt/haproxy/errorfiles/500.http
    errorfile 502 /opt/haproxy/errorfiles/502.http
    errorfile 503 /opt/haproxy/errorfiles/503.http
    errorfile 504 /opt/haproxy/errorfiles/504.http
    log global

frontend http
    bind *:80
    default_backend sfpay

backend sfpay	
    server server 127.0.0.1:8080


listen status
    bind *:1080
    stats refresh 30s
    stats uri /status
    stats realm HAProxy\ Stats
    stats auth admin:admin
```



### 6 启动

```shell
sbin/haproxy -f /opt/haproxy/conf/haproxy.cfg 
```



### 7 web

http://192.168.198.11:1080/status

admin/admin