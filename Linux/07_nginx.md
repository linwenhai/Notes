## nginx

#### 1 源码安装

```shell
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel pcre-devel
./configure \
--prefix=/app/nginx \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-stream
make && make install
```



#### 2 配置详解

```shell
nginx -s reload		#加载新配置
```

```nginx
main                                # 全局配置
events {                            # nginx工作模式配置
}
http {                              # http设置
    ....
    server {                        # 服务器主机配置
        	....
        	location {           	# 路由配置
           	 ....
        	}
    }
    upstream name {           		# 负载均衡配置
        ....
    }
}
```



#### 3 mail模块

```nginx
user root root;
worker_processes 2;
error_log logs/error.log;
pid logs/nginx.pid;
worker_rlimit_nofile 1024;

#user：用来指定nginx worker进程运行用户以及用户组
#worker_processes：指定nginx要开启的子进程数量
#error_log：定义错误日志文件的位置及输出级别
#pid：用来指定进程id的存储文件的位置
#worker_rlimit_nofile：用于指定一个进程可以打开最多文件数量的描述
```



#### 4 events模块

```nginx
event {
    worker_connections 1024;
    multi_accept on;
    use epoll;
}

#worker_connections： 指定最大可以同时接收的连接数量,max_clients = worker_processes * worker_connections/4
#multi_accept： 配置指定nginx在收到一个新连接通知后尽可能多的接受更多的连接
#use epoll： 配置指定了线程轮询的方法
```



#### 5 http模块



#### 6 server模块



#### 7  location模块

```nginx
location / {
    root    /nginx/www;
    index    index.php index.html index.htm;
}

#location /：表示匹配访问根目录
#root：用于指定访问根目录时，访问虚拟主机的web目录
#index：在不指定访问具体资源时，默认展示的资源文件列表
```



```nginx
#反向代理配置方式
location / {
    proxy_pass http://localhost:8080;
    proxy_set_header X-real-ip $remote_addr;
    proxy_set_header Host $http_host;
}

#正向代理配置方式
location / {
        resolver 192.168.241.2;
        proxy_pass $scheme://$http_host$request_uri;                                                 
}
```



#### 8  upstream模块

```nginx
upstream name {
    ip_hash;
    server 192.168.1.100:8000;
    server 192.168.1.101:8000;
}

#ip_hash：指定请求调度算法，默认是weight权重轮询调度，可以指定
#server host:port：分发服务器的列表配置
```

