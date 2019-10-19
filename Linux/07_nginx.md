## nginx

### 1 源码安装

```shell
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel pcre-devel
cd /app/nginx-1.14.2
./configure --prefix=/app/nginx
make && make install
```



### 2 添加新模块

如添加新模块stream

```shell
cd /app/nginx-1.14.2
./configure --prefix=/app/nginx --with-stream
make
cp -r ./objs/nginx /app/nginx/sbin/
```



### 3 模块详解

--with-http_ssl_module 		# SSL模块,支持Https
--with-stream						  #stream模块，支持负载均衡，代理



### 4 nginx启停

```shell
cd /app/nginx/sbin
./nginx -t			# 测试nginx程序是否正确
nohup ./nginx &		# 启动nginx
ps -ef|grep nginx|awk '{print $2}'|xargs kill -9	# 停止nginx
```



```shell
nginx -s reload		#加载新配置
```



### 5 配置详解

#### 5.1 mail模块

| 参数                 | 说明                                                         | 示例                              |
| -------------------- | ------------------------------------------------------------ | --------------------------------- |
| user                 | 运行nginx worker进程用户以及用户组                           | user  root root;                  |
| worker_processes     | nginx要开启的子进程数量                                      | worker_processes  1;              |
| error_log            | 错误日志文件的位置及输出级别，日志输出级别有debug、info、notice、warn、error、crit | error_log  logs/error.log  error; |
| pid                  | 进程id的存储文件的位置                                       | logs/nginx.pid;                   |
| worker_rlimit_nofile | nginx worker进程最大打开文件数                               | worker_rlimit_nofile 10240;       |



```nginx
worker_processes  2;
error_log  logs/error.log;
pid        logs/nginx.pid;
```





#### 5.2 events模块

| 参数               | 说明                                 | 示例                      |
| ------------------ | ------------------------------------ | ------------------------- |
| worker_connections | 每个进程允许的最多连接数，默认是1024 | worker_connections 10240; |
| use                | Nginx工作模式，默认是epoll           | use epoll;                |



```nginx
event {
    worker_connections 10240;
}
```





#### 5.3 http模块

##### 1】全局设置

| 参数                        | 说明                                                         | 示例                                   |
| --------------------------- | ------------------------------------------------------------ | -------------------------------------- |
| include                     | 对配置文件所包含的文件的设定                                 | include conf/mime.types;               |
| default_type                | HTTP类型                                                     | default_type application/octet-stream; |
| access_log                  | 日志文件的路径及格式                                         | access_log  logs/access.log  main;     |
| sendfile                    | 开启高效文件传输模式，默认为off                              | Sendfile on;                           |
| tcp_nopush                  | 依赖sendfile开启，调用tcp_cork方法，数据包不会马上传送出去，等到数据包最大时，一次性的传输出去，默认off | tcp_nopush on;                         |
| tcp_nodelay                 | 禁用Nagle的缓冲算法，并在数据可用时立即发送，默认为on        | tcp_nodelay on;                        |
| keepalive_timeout           | 客户端连接保持活动的超时时间                                 | keepalive_timeout 60;                  |
| client_max_body_size        | 允许客户端请求的最大的单个文件字节数                         | client_max_body_size 20m;              |
| client_header_buffer_size   | 客户端请求头的headerbuffer大小                               | client_header_buffer_size 32K;         |
| large_client_header_buffers | 客户端请求中较大的消息头的缓存最大数量和大小                 | large_client_header_buffers 4 32k;     |
| client_header_timeout       | 客户端请求头读取超时时间                                     | client_header_timeout 10;              |
| client_body_timeout         | 客户端请求主体读取超时时间                                   | client_body_timeout 10;                |
| send_timeout                | 响应客户端的超时时间                                         | send_timeout 10;                       |



```nginx
include       mime.types;
default_type  application/octet-stream;
access_log  logs/access.log  main;
sendfile        on;
#tcp_nopush     on;
keepalive_timeout  65;
```







##### 2】log_format设置

| 参数                    | 说明                                         | 示例                                                         |
| ----------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| $remote_addr            | 客户端地址                                   | 211.28.65.253                                                |
| $remote_user            | 客户端用户名称                               | --                                                           |
| $time_local             | 访问时间和时区                               | 18/Jul/2012:17:00:01 +0800                                   |
| $request                | 请求的URI和HTTP协议                          | "GET /article-10000.html HTTP/1.1"                           |
| $http_host              | 请求地址(IP或域名)                           | [www.it300.com 192.168.100.100](www.it300.com)               |
| $status                 | HTTP请求状态                                 | 200                                                          |
| $upstream_status        | upstream状态                                 | 200                                                          |
| $body_bytes_sent        | 发送给客户端文件内容大小                     | 1547                                                         |
| $http_referer           | url跳转来源                                  | https://www.baidu.com/                                       |
| $http_user_agent        | 用户终端浏览器等信息                         | "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; SV1; GTB7.0; .NET4.0C; |
| $ssl_protocol           | SSL协议版本                                  | TLSv1                                                        |
| $ssl_cipher             | 交换数据中的算法                             | RC4-SHA                                                      |
| $upstream_addr          | 后台upstream的地址，即真正提供服务的主机地址 | 10.10.10.100:80                                              |
| $request_time           | 整个请求的总时间                             | 0.205                                                        |
| $upstream_response_time | 请求过程中，upstream响应时间                 | 0.002                                                        |
| $http_x_forwarded_for   | 客户端的真实ip                               |                                                              |



```nginx
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
```



##### 3】upstream设置

| 参数         | 说明                             | 示例 |
| ------------ | -------------------------------- | ---- |
| ip_hash      | 基于Hash计算                     |      |
| server       | 反向服务地址和端口               |      |
| weight       | 权重                             |      |
| max_fails    | 失败多少次认为主机已挂掉则，踢出 |      |
| fail_timeout | 踢出后重新探测时间               |      |
| max_conns    | 允许最大连接数                   |      |



```nginx
upstream kibana_web {
    server 192.168.1.11:8080    weight=1  max_fails=2  fail_timeout=30;
    server 192.168.1.12:8080    weight=1  max_fails=2  fail_timeout=30;
    check interval=3000 rise=2 fall=5 timeout=1000 type=http;
	check_http_send "HEAD /mwmonitor/check.jsp HTTP/1.0\r\n\r\n";
	check_http_expect_alive http_2xx http_3xx;
}
```

>每个3秒检测一次，请求2次正常则标记realserver状态为up，如果检测5次都失败，则标记realserver的状态为down，超时时间为1秒。







##### 4】server全局设置

```nginx
listen       80;
server_name  localhost;

error_page   500 502 503 504  /50x.html;
```


##### 5】location设置

| 参数 | 说明                                                         | 示例               |
| ---- | ------------------------------------------------------------ | ------------------ |
| =    | 表示精确匹配，请求的url路径与后面的字符串完全相等时，才会命中 | location = /       |
| ~    | 表示该规则是使用正则定义的，区分大小写                       | location ~ \.php$  |
| ~*   | 表示该规则是使用正则定义的，不区分大小写                     | location ~* \.php$ |
| !~   | 区分大小写不匹配                                             |                    |
| !~*  | 不区分大小写不匹配                                           |                    |
| /    | 通用匹配，任何请求都会匹配到                                 |                    |
| ^~   | 前缀匹配，如该符号后面的字符被匹配上，则被默认为最佳匹配，即采用该规则 | location ^~ /blogs |



**location匹配优先级**：

1、等号类型，精确匹配
2、^~类型，前缀匹配，不支持正则，如该符号后面的字符匹配被匹配上，则被默认为最佳匹配，不再继续往下查找
3、~和~*类型，正则匹配
4、前缀匹配类型，如location / {}（表示任何以/开头的URL都匹配）或location /user {}，只不过找到合适了还会继续往下找，直到找到最长匹配



```nginx
location / {
	root   html;
	index  index.html index.htm;
}
```





反向代理：代理的是服务器，隐藏后端服务器地址。

```nginx
server {
    listen       80;
    server_name  www.123.com;

    location / {
        proxy_pass http://www.google.com;
        index  index.html index.htm index.jsp;
    }
}
```



正向代理：代理的是客户端，隐藏客户端地址。

```nginx
location / {
        resolver 192.168.241.2;
        proxy_pass $scheme://$http_host$request_uri;                               
}
```





##### 6】访问权限设置

auth_basic "The Kibana Monitor Center";
auth_basic_user_file /app/nginx/conf/.htpasswd;

yum install httpd-tools
htpasswd -c /app/nginx/conf/.htpasswd elk















