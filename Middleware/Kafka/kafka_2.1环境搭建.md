1 JDK



2 zookeeper



### 3 server.properties

修改如下内容

```
broker.id=1
log.dirs=/app/kafka/logs
zookeeper.connect=10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181
```



```shell
mkdir /app/kafka/logs
```





### 4 启动

```shell
/app/kafka/bin/kafka-server-start.sh -daemon /app/kafka/config/server.properties &
```



### 5 创建topic

```shell
/app/kafka/bin/kafka-topics.sh --create --zookeeper 10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181 --replication-factor 2 --partitions 1 --topic tttt
```

参数解释:
　　--replication-factor 2	复制两份
　　--partitions 1	创建1个分区
　　--topic tttt	topic 名称



查看已经存在的topic

```shell
cd /app/kafka/bin/
./kafka-topics.sh --list --zookeeper 10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181
```



### 6 删除topic

```shell
./kafka-topics.sh --delete --zookeeper 10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181 --topic tttt
```





查询集群描述

```shell
./kafka-topics.sh --describe --zookeeper 10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181
```



消费者列表查询

```shell
./kafka-topics.sh --zookeeper 10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181 --list
```



生成数据

```shell
./kafka-console-producer.sh -broker-list 10.203.197.48:9092,10.203.197.43:9092,10.203.197.113:9092 --topic tttt
```



消费生产数据

```shell
./kafka-console-consumer.sh --bootstrap-server 10.203.197.48:9092,10.203.197.43:9092,10.203.197.113:9092 --from-beginning --topic tttt
```



查看指定topic信息

```shell
./kafka-topics.sh --describe --zookeeper 10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181 --topic tttt
```

partiton： partion id  分区id
leader：当前负责读写的lead broker id ，就是server.properties的broker.id
replicas：当前partition的所有replication broker  list 
isr：relicas的子集，只包含出于活动状态的broker，离线或挂掉的broker不在此列表


