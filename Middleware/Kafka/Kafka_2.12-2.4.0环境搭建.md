## Kafka_2.11-2.1.1环境搭建

版本：kafka_2.11-2.1.1.tgz

### 1 JDK

```
export JAVA_HOME=/app/jdk1.8.0_201 
export PATH=$JAVA_HOME/bin:$PATH 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

```shell
java -version
```



### 2 Zookeeper

```
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/app/zookeeper/data
clientPort=2181
server.1=10.216.23.13:2888:3888
server.2=10.216.23.14:2888:3888
server.3=10.216.23.15:2888:3888
```

```shell
mkdir /app/zookeeper/data
/app/zookeeper/bin/zkServer.sh start
```





### 3 KAFKA环境

```shell
tar -xzf kafka_2.11-2.1.1.tgz
ln -s kafka_2.11-2.1.1 kafka
cd kafka
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

```shell
mkdir -p /app/kafka/logs
```





### 3 启动KAFKA

```shell
/app/kafka
bin/kafka-server-start.sh config/server.properties >/dev/null 2>&1 &

ps -ef|grep kafka|awk '{print $2}'|xargs kill -9
```



### 4 测试

```shell
# 创建topic
bin/kafka-topics.sh --create --zookeeper 10.216.23.13:2181 --replication-factor 1 --partitions 1 --topic test

# 查看主题列表
bin/kafka-topics.sh --list --zookeeper 10.216.23.13:2181

# 发送消息
bin/kafka-console-producer.sh --broker-list 10.216.23.13:9092 --topic test

# 启动消费者，标准输出
bin/kafka-console-consumer.sh --bootstrap-server 10.216.23.13:9092 --topic test --from-beginning

```



### 5 垂直集群

```shell
cp config/server.properties config/server-1.properties
cp config/server.properties config/server-2.properties
```

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

```shell
# 启动新节点
bin/kafka-server-start.sh config/server-1.properties &
bin/kafka-server-start.sh config/server-2.properties &
```



```shell
# 创建3分区1副本的主题test2
bin/kafka-topics.sh --create --zookeeper 10.216.23.13:2181 --replication-factor 1 --partitions 3 --topic test2

# 查看主题test2信息
bin/kafka-topics.sh --describe --zookeeper 10.216.23.13:2181 --topic test2

# 查看消费组信息
bin/kafka-consumer-groups.sh --bootstrap-server 10.216.23.13:9092 --describe --group my-group

bin/kafka-consumer-groups.sh --bootstrap-server 10.216.23.13:9092 --list





```

