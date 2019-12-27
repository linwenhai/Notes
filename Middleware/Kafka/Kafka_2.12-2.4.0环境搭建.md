## Kafka_2.12-2.4.0环境搭建

版本：kafka_2.12-2.4.0.tgz

### 1 JDK和zookeeper环境





### 2 KAFKA环境

```shell
tar -xzf kafka_2.12-2.4.0.tgz
cd kafka_2.12-2.4.0
```

server.properties

修改如下内容

```
broker.id=1
listeners=PLAINTEXT://10.216.23.13:9092
log.dirs=/app/kafka/logs
zookeeper.connect=10.216.23.13:2181,10.216.23.14:2181,10.216.23.15:2181
```

其他节点修改：

broker.id和listeners



mkdir -p /app/kafka/logs



### 3 启动KAFKA

```shell
/app/kafka/bin/kafka-server-start.sh /app/kafka/config/server.properties >/dev/null 2>&1 &


ps -ef|grep kafka|awk '{print $2}'|xargs kill -9
```



### 4 创建topic

```shell
bin/kafka-topics.sh --create --bootstrap-server 10.216.23.13:9092 --replication-factor 1 --partitions 1 --topic test


bin/kafka-topics.sh --create --bootstrap-server 10.216.23.13:9092 --replication-factor 3 --partitions 1 --topic test-2

```

参数解释: 　　

- replication-factor 1 复制1份 　　
- partitions 1 创建1个分区 　　
- topic test topic名称



### 5 测试

```shell
# 发送一些消息到topic
bin/kafka-console-producer.sh --broker-list 10.216.23.13:9092 --topic test
```



```shell
# 将消息转储到标准输出
bin/kafka-console-consumer.sh --bootstrap-server 10.216.23.13:9092 --topic test --from-beginning
```



### 6 查询

1】查看已经存在的topic

```shell
bin/kafka-topics.sh --list --bootstrap-server 10.216.23.13:9092
```



2】查看主题

```shell
bin/kafka-topics.sh --describe --bootstrap-server 10.216.23.13:9092 --topic test
```



3】查询集群描述

```
bin/kafka-topics.sh --describe --zookeeper 10.216.23.13:2181
```



4】消费者列表查询

```
bin/kafka-topics.sh --zookeeper 10.216.23.13:2181 --list
```





1 创建topic
bin/kafka-topics.sh --create --bootstrap-server 10.216.23.13:9092 --replication-factor 1 --partitions 3 --topic mytest


2 查看topic的详细信息
bin/kafka-topics.sh --describe --bootstrap-server 10.216.23.13:9092 --topic mytest


3 创建producer
节点1
bin/kafka-console-producer.sh --broker-list 10.216.23.13:9092 --topic mytest


4 创建consumer1,consumer12，指定组
节点2
bin/kafka-console-consumer.sh --bootstrap-server 10.216.23.13:9092 --topic mytest --consumer-property group.id=group_mytest
节点3
bin/kafka-console-consumer.sh --bootstrap-server 10.216.23.13:9092 --topic mytest --consumer-property group.id=group_mytest