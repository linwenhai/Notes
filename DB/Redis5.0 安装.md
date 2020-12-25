## Redis5.0 安装

### 1 Redis简介

Redis是一个开源的使用C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。

Redis 支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。

Redis应用场景：

- 1】缓存热数据；
- 2】计数器；
- 3】队列
- 4】位操作（大数据处理）
- 5】分布式锁与单线程机制
- 6】最新列表
- 7】排行榜

 

### 2 安装redis

Centos版本：7.8

Redis版本：5.0

官网下载地址：http://download.redis.io/releases/redis-5.0.0.tar.gz

```bash
yum install gcc-c++
cd /opt
tar -zxvf redis-5.0.0.tar.gz
cd redis-5.0.0/
make
make install PREFIX=/opt/redis
```



```bash
mkdir -p /opt/redis/conf
cp /opt/redis-5.0.0/redis.conf /opt/redis/conf
vi /opt/redis/conf/redis.conf
--------------------------
bind 192.168.100.11
port 8080
daemonize yes
protected-mode no
requirepass 123456    # 设置密码
--------------------------
```



```bash
bin/redis-server conf/redis.conf	# 启动redis
bin/redis-cli shutdown				# 关闭redis
ps -ef|grep redis					# 查看进程
```



```bash
# 链接redis
bin/redis-cli -h 192.168.100.11 -p 8080 -a 123456
192.168.100.11:8080> info
```



### 3 Redis常用命令

```bash
# 设置key-value
bin/redis-cli -h 192.168.100.11 -p 8080 -a 123456 set "abc" "123456"
 
# 查看key
bin/redis-cli -h 192.168.100.11 -p 8080 -a 123456 get "abc"
 
# 查看key是否存在
bin/redis-cli -h 192.168.100.11 -p 8080 -a 123456 keys "abc"
bin/redis-cli -h 192.168.100.11 -p 8080 -a 123456 exists "abc"
 
# 删除key
bin/redis-cli -h 192.168.100.11 -p 8080 -a 123456 del "abc"
```





