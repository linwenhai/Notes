## hadoop-2.8.5集群安装

### 1 各主机IP规划

| 主机名 | IP地址         | 操作系统  | 安装软件               | 运行进程                                                 |
| ------ | -------------- | --------- | ---------------------- | -------------------------------------------------------- |
| nn01   | 192.168.100.11 | centos7.2 | jdk、hadoop、zookeeper | NameNode、DFSZKFailoverController(zkfc)、ResourceManager |
| nn02   | 192.168.100.12 | centos7.2 | jdk、hadoop、zookeeper | NameNode、DFSZKFailoverController(zkfc)、ResourceManager |
| dn01   | 192.168.100.13 | centos7.2 | jdk、hadoop、zookeeper | DataNode、NodeManager、JournalNode、QuorumPeerMain       |
| dn02   | 192.168.100.14 | centos7.2 | jdk、hadoop、zookeeper | DataNode、NodeManager、JournalNode、QuorumPeerMain       |
| dn03   | 192.168.100.15 | centos7.2 | jdk、hadoop、zookeeper | DataNode、NodeManager、JournalNode、QuorumPeerMain       |



### 2 配置hosts信息

>添加如下内容
>
>192.168.100.11 nn01
>192.168.100.12 nn02
>192.168.100.13 dn01
>192.168.100.14 dn02
>192.168.100.15 dn03



### 3 创建用户和组

```shell
useradd hdfs
passwd hdfs
```



### 4 配置SSH免密登陆

```shell
# 生成rsa秘钥，在2个nn节点执行
ssh-keygen -t rsa

# 查看公钥，在2个nn节点执行
cat ~/.ssh/id_rsa.pub

# 将2个nn上的rsa公钥复制到文件authorized_keys，在所有nn和dn节点执行
vi ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```



### 5 配置JDK

```shell
cd /app
tar -zxvf jdk-8u201-linux-x64.tar.gz
ln -s dk-8u201-linux-x64 jdk
vi /etc/profile
```

>添加环境变量
>
>export JAVA_HOME=/app/jdk
>export PATH=$JAVA_HOME/bin:$PATH

```shell
source /etc/profile
java -version
```



### 6 配置zookeeper

zookeeper节点数为奇数。

```shell
cd /app
tar -zxvf zookeeper-3.4.6.tar.gz
ln -s zookeeper-3.4.6 zookeeper

cd /app/zookeeper/conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg
```

>修改如下内容
>
>dataDir=/data/zookeeper
>server.1=nn01:2888:3888
>server.2=nn02:2888:3888
>server.3=dn01:2888:3888
>server.4=dn02:2888:3888
>server.5=dn03:2888:3888



在每个节点的dataDir目录创建文件myid，id值为数字，如

| 节点 | id值 |
| ---- | ---- |
| nn01 | 1    |
| nn02 | 2    |
| dn01 | 3    |
| dn02 | 4    |
| dn03 | 5    |

```shell
cat 1 /data/zookeeper/myid
```



```shell
#启动ZK
/app/zookeeper/bin/zkServer.sh start

# 测试zk
/app/zookeeper/bin/zkCli.sh
[zk: localhost:2181(CONNECTED) 1] ls /

jps
```

>显示结果：
>
>420257 QuorumPeerMain





### 7 安装hadoop

```shell
cd /app
tar -zxvf hadoop-2.8.5.tar.gz
ln -s hadoop-2.8.5 hadoop
vi /etc/profile
```

>------添加如下内容---------------
>export HADOOP_HOME=/app/hadoop
>export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH

```shell
source /etc/profile
```



### 8 hadoop-env.sh文件

>添加变量
>
>export HADOOP_NAMENODE_OPTS=" -Xms1024m -Xmx1024m -XX:+UseParallelGC"
>export HADOOP_DATANODE_OPTS=" -Xms1024m -Xmx1024m"
>export HADOOP_LOG_DIR=/logs/hadoop

-Xms 为jvm启动时分配的内存
-Xmx 为jvm运行过程中分配的最大内存
-Xss 为jvm启动的每个线程分配的内存大小



### 9 core-site.xml文件

```xml
<configuration>
 <property>
     <name>fs.defaultFS</name>
     <value>hdfs://mycluster/value>
 </property>
</configuration>
```

>9000端口是早期版本的hadoop，后改为8020



### 10 hdfs-site.xml配置文件

```xml
<configuration>
 <property>
     <name>dfs.namenode.name.dir</name>
     <value>file:///data/hadoop/nn</value>
 </property>
 <property>
     <name>dfs.datanode.data.dir</name>
     <value>file:///data/hadoop/dn</value>
 </property>
 <property>
     <name>dfs.replication</name>
     <value>3</value>
 </property>
</configuration>    
```



### 11 配置Hadoop HA

1】配置nameservice ID

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.nameservices</name>
     <value>mycluster</value>
 </property>
```

2】配置含nameservice ID的namenode

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.ha.namenodes.hadoopha</name>
     <value>nn1,nn2</value>
 </property>
```

3】配置namenode的rpc访问地址

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.namenode.rpc-address.mycluster.nn1</name>
     <value>nn01:8020</value>
 </property>
 <property>
     <name>dfs.namenode.rpc-address.mycluster.nn1</name>
     <value>nn02:8020</value>
 </property>
```

4】配置namenode的http访问地址

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.namenode.http-address.mycluster.nn1</name>
     <value>nn01:50070</value>
 </property>
 <property>
     <name>dfs.namenode.http-address.mycluster.nn1</name>
     <value>nn02:50070</value>
 </property>
```

5】配置journalnode地址和数据文件存储位置

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.namenode.shared.edits.dir</name>
     <value>qjournal://nn01:8485;nn02:8485;nn03:8485/mycluster</value>
 </property>
 <property>
      <name>dfs.journalnode.edits.dir</name>
      <value>/data/hadoop/journal</value>
 </property>
```

>journalnode用于同步2个namenode之间的操作，防止脑裂，建议journalnode节点放在zookeeper节点上，节点为奇数。

6】配置dfs客户端

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.client.failover.proxy.provider.mycluster</name>     	
     <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
 </property>
```

7】配置kill掉namenode方式

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.ha.fencing.methods</name>
     <value>sshfence</value>
 </property>
 <property>
      <name>dfs.ha.fencing.ssh.private-key-files</name>
      <value>/home/hdfs/.ssh/id_rsa</value>
 </property>
```

8】配置ZKFC心跳

 在core-site.xml文件配置

```xml
 <property>
    <name>ha.zookeeper.session-timeout.ms</name>
    <value>15000</value>
 </property>
 <property>
      <name>ha.zookeeper.quorum</name>
      <value>nn01:2181,nn02:2181,dn01:2181,dn02:2181,dn03:2181</value>
 </property>
```

9】配置自动failover

 在hdfs-site.xml文件配置

```xml
 <property>
     <name>dfs.ha.automatic-failover.enabled</name>
     <value>true</value>
 </property>
```

>auto failover基于zookeeper的，需添加zk集群地址。





### 12 创建目录

```shell
mkdir -p /data/hadoop/nn
mkdir -p /data/hadoop/dn
mkdir -p /logs/hadoop
mkdir -p /data/zookeeper
mkdir -p /logs/zookeeper
mkdir -p /data/hadoop/journal
chown -R hdfs:hdfs /data
chown -R hdfs:hdfs /logs
```



### 13 初始化hadoop

```shell
#初始化namenode，在nn01执行
hdfs namenode -format

#同步namenode，在nn02执行
hdfs namenode -bootstrapStandby	

#初始化journalnode
hdfs namenode -initializeSharedEdits

#初始化zookeeper
hdfs zkfc -formatZK
```



### 14 hadoop启动

```shell
#journalnode启动
sbin/hadoop-daemon.sh start journalnode
#journalnode停止
sbin/hadoop-daemon.sh stop journalnode
jps

#namenode启动
sbin/hadoop-daemon.sh --scriprt hdfs start namenode
#namenode停止
sbin/hadoop-daemon.sh --scriprt hdfs stop namenode
jps

#zkfc启动
sbin/hadoop-daemon.sh --scriprt hdfs start zkfc

#datanode启动
sbin/hadoop-daemon.sh --scriprt hdfs start datanode
#datanode停止
sbin/hadoop-daemon.sh --scriprt hdfs stop datanode
jps
```



### 15 手动failover

```shell
#nn01切换为active状态
hdfs haadmin -failover nn02 nn01
```











