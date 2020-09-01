## Redis常用命令

### 1 设置key-value

```shell
/app/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 set "aaa2d2df2" "bbb"
```



### 2 查看key

```shell
/app/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 get "aaa2d2df2"
```



### 3 查看某个key是否存在

```shell
/usr/local/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 keys "aaa2d2df2"     
/usr/local/redis/bin/redis-cli -h 127.0.0.1 -p 8080 -a 123456 exists "aaa2d2df2"
```



