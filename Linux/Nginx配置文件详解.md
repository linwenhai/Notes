## Nginx配置文件详解

### 一、Mail模块

| 参数                 | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| user                 | 运行nginx worker进程用户以及用户组                           |
| worker_processes     | nginx要开启的子进程数量                                      |
| error_log            | 错误日志文件的位置及输出级别，日志输出级别有debug、info、notice、warn、error、crit |
| pid                  | 进程id的存储文件的位置                                       |
| worker_rlimit_nofile | 进程最大打开文件数                                           |

```bash
user  root root;
worker_processes  2;
error_log  logs/error.log;
pid        logs/nginx.pid;
```



### 二、Events模块

| 参数               | 描述                                 |
| :----------------- | :----------------------------------- |
| worker_connections | 每个进程允许的最多连接数，默认是1024 |
| use                | Nginx工作模式，默认是epoll           |

```bash
event {
    worker_connections 10240;
}
```



### 三、Http模块

| 参数                        | 描述                                                         |
| :-------------------------- | :----------------------------------------------------------- |
| include                     | 对配置文件所包含的文件的设定                                 |
| default_type                | HTTP类型                                                     |
| log_format                  | 日志格式                                                     |
| access_log                  | 日志文件的路径及格式                                         |
| sendfile                    | 开启高效文件传输模式，默认为off                              |
| tcp_nopush                  | 依赖sendfile开启，调用tcp_cork方法，数据包不会马上传送出去，等到数据包最大时，一次性的传输出去，默认off |
| tcp_nodelay                 | 禁用Nagle的缓冲算法，并在数据可用时立即发送，默认为on        |
| keepalive_timeout           | 客户端连接保持活动的超时时间                                 |
| client_max_body_size        | 允许客户端请求的最大的单个文件字节数                         |
| client_header_buffer_size   | 客户端请求头的headerbuffer大小                               |
| large_client_header_buffers | 客户端请求中较大的消息头的缓存最大数量和大小                 |
| client_header_timeout       | 客户端请求头读取超时时间                                     |
| client_body_timeout         | 客户端请求主体读取超时时间                                   |
| send_timeout                | 响应客户端的超时时间                                         |

```bash
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```



#### 1 log_format

| 参数                    | 描述                         | 示例                                                         |
| :---------------------- | :--------------------------- | :----------------------------------------------------------- |
| $remote_addr            | 客户端地址                   | 219.227.111.255                                              |
| $remote_user            | 客户端用户名称               |                                                              |
| $time_local             | 访问时间和时区               | 18/Jul/2014:17:00:01 +0800                                   |
| $request                | 请求的URI和HTTP协议          | “GET /article-10000.html HTTP/1.1”                           |
| $status                 | HTTP请求状态                 | 200                                                          |
| $body_bytes_sent        | 发送给客户端文件内容大小     | 1547                                                         |
| $http_referer           | url跳转来源                  | https://www.google.com/                                      |
| $http_user_agent        | 用户终端浏览器等信息         | “Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; SV1; GTB7.0; .NET4.0C; |
| $http_x_forwarded_for   |                              |                                                              |
| $upstream_status        | upstream状态                 | 200                                                          |
| $upstream_addr          | 后台upstream的地址           | 10.36.10.80:80                                               |
| $request_time           | 整个请求的总时间             | 0.165                                                        |
| $upstream_response_time | 请求过程中，upstream响应时间 | 0.002                                                        |

```bash
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for" '
		          '"$upstream_addr" $request_time $upstream_response_time';
    access_log  logs/access.log  main;
```



#### 2 upstream

| 参数         | 描述                             |
| :----------- | :------------------------------- |
| ip_hash      | 基于Hash计算                     |
| server       | 反向服务地址和端口               |
| weight       | 权重                             |
| max_fails    | 失败多少次认为主机已挂掉则，踢出 |
| fail_timeout | 踢出后重新探测时间               |
| max_conns    | 允许最大连接数                   |

```bash
upstream kibana_web {
    server 192.168.1.11:8080  max_fails=3  fail_timeout=30s;
    server 192.168.1.12:8080  max_fails=3  fail_timeout=30s;
}
 
server {
    --snip--
    location / {
        proxy_pass http://kibana_web;
        proxy_set_header X-real-ip $remote_addr;
        proxy_set_header Host $http_host;
    }
    --snip--
}
```



#### 3 location

| 参数    | 说明                                           |
| ------- | ---------------------------------------------- |
| =       | 表示精确匹配                                   |
| ^~      | 表示匹配URL路径，以xx开头                      |
| ~       | 正则匹配，区分大小写，以xx结尾                 |
| ~*      | 正则匹配，不区分大小写，以xx结尾               |
| !~和!~* | 正则不匹配，区分大小写和不区分大小写，以xx结尾 |
| /       | 通用匹配，任何请求都会匹配到。                 |

**【匹配顺序】：**

1. 首先精确匹配 =
2. 其次以xx开头匹配^~
3. 然后是按文件中顺序的正则匹配
4. 最后是交给 / 通用匹配

当有匹配成功时候，停止匹配，按当前匹配规则处理请求。

```bash
location = / {
   #规则A
}
location = /login {
   #规则B
}
location ^~ /static/ {
   #规则C
}
location ~* \.png$ {
   #规则D
}
location / {
   #规则E
}
```

>访问 http://localhost/ 将匹配规则A；
>访问 http://localhost/login 将匹配规则B；
>访问 http://localhost/static/a.html 将匹配规则C；
>访问 http://localhost/a.png 则匹配规则D；
>访问 http://localhost/category/id/1111 则匹配到规则E，因为以上规则都不匹配。



### 四、Stream模块

```bash
stream {
    log_format proxy '$remote_addr [$time_local] '
			'$protocol $status $bytes_sent $bytes_received '
			'$session_time "$upstream_addr" '
			'"$upstream_bytes_sent" "$upstream_bytes_received" $upstream_connect_time';
	
	access_log logs/tcp-access.log proxy
	
    upstream cmbchina {
        server www.cmbchina.com:443;
    }
    server {
        listen 8081;
        proxy_connect_timeout 8s;
        proxy_timeout 24h;
        proxy_pass cmbchina;
    }
}
```



