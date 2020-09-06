## Nginx环境搭建

版本：nginx-1.14.2

### 1 源码安装

```shell
yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel pcre-devel
cd /opt/nginx-1.14.2
./configure --prefix=/opt/nginx 
make && make install
```

### 2 添加新模块

```shell
#查看版本和安装模块 
sbin/nginx -V

#添加新模块 
cd /opt/nginx-1.14.2 
./configure --prefix=/app/nginx --with-stream 
make 
cp -r ./objs/nginx /app/nginx/sbin/
```

### 3 启停

```shell
cd /app/nginx
sbin/nginx -t                # 测试nginx程序是否正确
nohup sbin/nginx &           # 启动nginx
ps -ef|grep nginx|awk '{print $2}'|xargs kill -9    # 停止nginx
sbin/nginx -s reload        # 加载新配置
```

### 4 模块详解

|                        | 描述                           |
| ---------------------- | ------------------------------ |
| --with-http_ssl_module | SSL模块,支持Https              |
| --with-stream          | stream模块，支持负载均衡、代理 |

### 5 访问权限设置

```shell
# 配置文件添加如下内容
auth_basic "The Kibana Monitor Center";
auth_basic_user_file /app/nginx/conf/.htpasswd;
```

```shell
yum install httpd-tools
# 创建elk用户和密码
htpasswd -c /app/nginx/conf/.htpasswd elk
```

