





1 查看状态

```shell
bin/redis-cli -h 10.117.227.111 -p 8001
10.117.227.111:8001> info            #查看哨兵状态
```

```shell
bin/redis-cli -h 10.117.227.111 -p 8080 -a o1GCaEAruN
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







