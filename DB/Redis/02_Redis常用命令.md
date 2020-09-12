## Redis常用命令

### 1 设置key-value

```shell
/opt/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 set "abc" "123456"
```



### 2 查看key

```shell
/opt/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 get "abc"
```



### 3 查看key是否存在

```shell
/opt/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 keys "abc"     
/opt/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 exists "abc"
```



### 4 删除key

```shell
/opt/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 del "abc"   
```



