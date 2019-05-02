redis



#### 1 查看状态

```shell
./redis-cli -h 10.117.227.111 -p 8001
10.117.227.111:8001> info			#查看哨兵状态
```

```shell
/redis-cli -h 10.117.227.111 -p 8080 -a o1GCaEAruN
10.117.227.111:8080> info			#查看实例状态
```



#### 2 Redis启动/停止

```shell
/app/redis/bin/redis-server /app/redis/conf/<redis>.conf
/app/redis/bin/redis-cli –h <IP> -p <PORT> -a <PASSWD> shutdown
```



#### 3 sentinel启动

```shell
redis-server /path/to/sentinel.conf --sentinel
```



#### 4 操作KEY

```shell
./redis-cli -h 10.117.227.111 -p 8080 -a o1GCaEAruN
10.117.227.111:8080> del key值		#删除KEY
10.116.117.117:8080> exists INVOICE_STATE_5132736034_201711   	#查询KEY是否存在
10.116.117.117:8080> get INVOICE_STATE_5150398945_201712		#查询KEY值
```





