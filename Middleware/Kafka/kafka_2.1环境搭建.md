版本：kafka_2.12-2.4.0.tgz



### 1 JDK环境

1.8



### 3 KAFKA环境

```shell
tar -xzf kafka_2.12-2.4.0.tgz
cd kafka_2.12-2.4.0
```



server.properties

修改如下内容

```
broker.id=1
log.dirs=/app/kafka/logs
zookeeper.connect=10.203.197.48:2181,10.203.197.43:2181,10.203.197.113:2181
```



```shell
mkdir /app/kafka/logs
```





### 4 启动KAFKA

```SHELL
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties
```









### 5 创建topic

```shell
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```

参数解释:
　　--replication-factor 1	复制1份
　　--partitions 1	创建1个分区
　　--topic test	topic名称



查看已经存在的topic

```shell
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```



### 6 生成数据

```shell
# 测试生成数据到topic
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```



### 7 消费数据

```shell
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```





### 8 启动另2个节点（同OS上）

```shell
cp config/server.properties config/server-1.properties
cp config/server.properties config/server-2.properties
```



修改配置文件，broker.id 需每个节点不一样

```
config/server-1.properties:
    broker.id=1
    listeners=PLAINTEXT://:9093
    log.dirs=/tmp/kafka-logs-1
 
config/server-2.properties:
    broker.id=2
    listeners=PLAINTEXT://:9094
    log.dirs=/tmp/kafka-logs-2
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


