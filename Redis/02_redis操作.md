1 查看状态

```shell
bin/redis-cli -h 10.116.117.140 -p 8001
10.117.227.111:8001> info            #查看哨兵状态
```

```shell
bin/redis-cli -h 10.116.117.140 -p 8080 -a u7m%@w9rrdqz3aly
10.117.227.111:8080> info            #查看实例状态
```



2 实例启停

```shell
bin/redis-server /app/redis/conf/<redis>.conf
bin/redis-cli –h <IP> -p <PORT> -a <PASSWD> shutdown
```



3 sentinel启停

```shell
bin/redis-server /path/to/sentinel.conf --sentinel
```



4 操作key

```shell
bin/redis-cli -h 10.117.227.111 -p 8080 -a o1GCaEAruN
```



5 查询key是否存在

```shell
10.116.117.117:8080> exists INVOICE_STATE_5132736034_201711     
```



6 查询key值

```shell
10.116.117.117:8080> get INVOICE_STATE_5150398945_201712
```



7 删除key值

```shell
10.117.227.111:8080> del <key值>
```

