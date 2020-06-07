## Zookeeper安装

### 1 安装

```shell
# zookeeper集群节点数为奇数。
cd /opt
tar -zxvf zookeeper-3.4.6.tar.gz
ln -s zookeeper-3.4.6 zookeeper
cd /opt/zookeeper/conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg
```

```
#修改如下内容
dataDir=/opt/zookeeper/data
server.1=nn01:2888:3888
server.2=nn02:2888:3888
server.3=dn01:2888:3888
server.4=dn02:2888:3888
server.5=dn03:2888:3888
```



```shell
#在每个节点的dataDir目录创建文件myid，id值为数字，各节点不一样
echo "1" > /opt/zookeeper/zookeeper/myid
```



### 2 启停

```shell
#启动ZK
/opt/zookeeper/bin/zkServer.sh start
/opt/zookeeper/bin/zkServer.sh status
jsp
```



```shell
# 测试zk
/opt/zookeeper/bin/zkCli.sh
[zk: localhost:2181(CONNECTED) 1] ls /
```



### 3 ZK命令

```shell
ls  /   #查找根目录
create /test abc   #创建节点并赋值
get /test   #获取指定节点的值
set /test cb  #设置已存在节点的值
rmr /test  #递归删除节点
delete /test/test01  #删除不存在子节点的节点
```





###  4 zoo.cfg参数

- tickTime =2000：通信心跳数Zookeeper 使用的基本时间，服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime时间就会发送一个心跳，时间单位为毫秒它用于心跳机制，并且设置最小的 session 超时时间为两倍心跳时间。(session的最小超时时间是2*tickTime)；
- initLimit =10：主从初始通信时限，集群中的 Follower 跟随者服务器与 Leader 领导者服务器之间初始连接时能容忍的最多心跳数（tickTime的数量），用它来限定集群中的 ZK 服务器连接到 Leader 的时限；
- syncLimit =5：主从同步通信时限，集群中 Leader 与 Follower 之间的最大响应时间单位，假如响应超过syncLimit * tickTime，Leader 认为 Follwer 死掉，从服务器列表中删除 Follwer；
- dataDir：数据文件目录+数据持久化路径；
- clientPort =2181：客户端连接端口



