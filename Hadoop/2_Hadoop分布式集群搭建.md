## Hadoop分布式集群搭建

版本：hadoop-2.8.5.tar.gz

### 1 集群节点

| 10.1.1.15 |          | datanode | zookeeper | JournalNode |                 | nodemanager |
| --------- | -------- | -------- | --------- | ----------- | --------------- | ----------- |
| 10.1.1.14 |          | datanode | zookeeper | JournalNode |                 | nodemanager |
| 10.1.1.13 |          | datanode | zookeeper | JournalNode |                 | nodemanager |
| 10.1.1.12 | namenode |          | zookeeper | JournalNode | resourcemanager |             |
| 10.1.1.11 | namenode |          | zookeeper | JournalNode | resourcemanager |             |

### 2 OS配置

#### 2.1 关闭iptable与selinux

root用户配置

```bash
service iptables stop
chkconfig iptables off
chkconfig iptables --list
vi /etc/sysconfig/selinux
```

>SELINUX=disabled

#### 2.2 配置hosts文件

root用户配置

```bash
vi /etc/hosts
```

>10.1.1.15	hadoop5
>10.1.1.14	hadoop4
>10.1.1.13	hadood3
>10.1.1.12	hadoop2
>10.1.1.11	hadoop1

#### 2.3 创建用户和目录

root用户配置

```bash
groupadd hdfs
useradd -g hdfs hdfs
passwd hdfs
chown -R hdfs:hdfs /opt
mkdir -p /data/hadoop/tmp
mkdir -p /data/hadoop/journal
mkdir -p /data/hadoop/hdfs/namenode
mkdir -p /data/hadoop/hdfs/datanode
mkdir -p /data/zookeeper/data
chown -R hdfs:hdfs /data
```

#### 2.4 配置JDK

hdfs用户配置

```bash
cd /opt
tar -zxvf jdk1.8.0_161.tar.gz
ln -s jdk1.8.0_161 jdk
vi ~/.bashrc
```

>export JAVA_HOME=/opt/jdk
>export ZOOKEEPER_HOME=/opt/hadoop
>export HADOOP_HOME=/opt/hadoop
>export PATH=.:$JAVA_HOME/bin:$ZOOKEEPER_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH

#### 2.5 配置ssh免密登录

hdfs用户配置

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
ssh localhost
```

将其余节点的id_dsa.pub文件内容添加到新文件authorized_keys，authorized_keys拷贝至所有服务器hdfs用户目录~/.ssh/。

###  

### 3 hadoop安装

hdfs用户配置

```bash
cd /opt
tar -zxvf hadoop-2.8.5.tar.gz
ln -s hadoop-2.8.5 hadoop
```

#### 3.1 hadoop-env.sh配置文件

```bash
$HADOOP_HOME/etc/hadoop/hadoop-env.sh
$HADOOP_HOME/etc/hadoop/mapred-env.sh
$HADOOP_HOME/etc/hadoop/yarn-env.sh
```

>export JAVA_HOME=/opt/jdk

#### 3.2 core-site.xml配置文件

$HADOOP_HOME/etc/hadoop/core-site.xml

```xml
<configuration>
	<!--文件系统名称-->
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://hdfs</value>
	</property>
	
	<!--临时文件存放路径-->
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/data/hadoop/tmp</value>
	</property>
	
	<!--Zookeeper地址-->
	<property>
		<name>ha.zookeeper.quorum</name>
		<value>hadoop1:2181,hadoop2:2181,hadoop3:2181,hadoop4:2181,hadoop5:2181</value>
	</property>
		
	<property>
		<name>io.file.buffer.size</name>
		<value>131072</value>
	</property> 
</configuration>
```

#### 3.3 hdfs-site.xml配置文件

```XML
<configuration>
	<!-- 集群名和节点 -->
	<property>
		<name>dfs.nameservices</name>
		<value>hdfs</value>
	</property>
	<property>
		<name>dfs.ha.namenodes.hdfs</name>
		<value>nn1,nn2</value>
	</property>
	
	<!-- namenode RPC和HTTP 地址 -->
	<property>
		<name>dfs.namenode.rpc-address.hdfs.nn1</name>
		<value>hadoop1:8020</value>
	</property>
	<property>
		<name>dfs.namenode.rpc-address.hdfs.nn2</name>
		<value>hadoop2:8020</value> 
	<property>
		<name>dfs.namenode.http-address.hdfs.nn1</name>
		<value>hadoop1:50070</value>
	</property>
	<property>
		<name>dfs.namenode.http-address.hdfs.nn2</name>
		<value>hadoop2:50070</value>
	</property> 
	
	<!-- JournalNode配置 -->
	<property>
		<name>dfs.namenode.shared.edits.dir</name>
		<value>qjournal://hadoop1:8485;hadoop2:8485;hadoop3:8485;hadoop4:8485;hadoop5:8485/hdfs</value> 
	</property>
	<property>
		<name>dfs.journalnode.edits.dir</name>
		<value>/data/hadoop/journal</value>
	</property>
	
	<!-- namenode HA自动切换配置 -->
	<property>
		<name>dfs.ha.automatic-failover.enabled</name>
		<value>true</value>
	</property>
	<property>
		<name>dfs.client.failover.proxy.provider.hdfs</name>
		<value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
	</property>
	<property>
		<name>dfs.ha.fencing.methods</name>
		<value>sshfence</value>
	</property>
	<property>
		<name>dfs.ha.fencing.ssh.private-key-files</name>
		<value>/home/hdfs/.ssh/id_dsa</value>
	</property>
	<property>
		<name>dfs.ha.fencing.ssh.connect-timeout</name>
		<value>30000</value>
	</property>
	
	<!-- 块副本 -->
	<property>
		<name>dfs.replication</name>
		<value>3</value>
	</property>
	
	<!-- nameNode与datanode存储目录 --> 
	<property>
		<name>dfs.namenode.name.dir</name>
		<value>file:///data/hadoop/hdfs/namenode</value>
	</property> 
	<property>
		<name>dfs.datanode.data.dir</name>
		<value>file:///data/hadoop/hdfs/datanode</value>
	</property> 
</configuration>
```

#### 3.4 mapred-site.xml配置文件

 $HADOOP_HOME/etc/hadoop/mapred-site.xml

```xml
<configuration>
	<!--指定Hadoop的MapReduce运行在YARN环境-->
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
	<!--MapReduceJobHistoryServer--> 
	<property>
		<name>mapreduce.jobhistory.address</name>
		<value>hadoop1:10020</value>
	</property> 
	<property>
		<name>mapreduce.jobhistory.webapp.address</name>
		<value>hadoop1:19888</value>
	</property>
</configuration>
```

#### 3.5 yarn-site.xml配置文件

```XML
<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	
	<!-- yarnHA -->
	<property>
		<name>yarn.resourcemanager.ha.enabled</name>
		<value>true</value>
	</property>
	<property>
		<name>yarn.resourcemanager.cluster-id</name>
		<value>cluster1</value>
	</property>
	<property>
		<name>yarn.resourcemanager.ha.rm-ids</name>
		<value>rm1,rm2</value>
	</property>
	<property>
		<name>yarn.resourcemanager.hostname.rm1</name>
		<value>hadoop1</value>
	</property>
	<property>
		<name>yarn.resourcemanager.hostname.rm2</name>
		<value>hadoop2</value>
	</property>
	<property>
		<name>yarn.resourcemanager.webapp.address.rm1</name>
		<value>hadoop1:8088</value>
	</property>
	<property>
		<name>yarn.resourcemanager.webapp.address.rm2</name>
		<value>hadoop2:8088</value>
	</property>
	
	<!-- zookeeper -->
	<property>
		<name>yarn.resourcemanager.zk-address</name>
		<value>hadoop1:2181,hadoop2:2181,hadoop3:2181,hadoop4:2181,hadoop5:2181</value>
	</property>
	<property>
		<name>yarn.resourcemanager.ha.automatic-failover.enabled</name>
		<value>ture</value>
	</property>
</configuration>
```

#### 3.6 slaves配置文件

$HADOOP_HOME/etc/hadoop/slaves

```bash
# 添加datanode
hadoop3
hadoop4
hadoop5
```



### 4 zookeeper安装

```bash
cd /opt
tar -zxvf apache-zookeeper-3.5.5-bin.tar.gz
ln -s apache-zookeeper-3.5.5-bin zookeeper
$ZOOKEEPER_HOME/conf/zoo.cfg
```

>tickTime=2000
>initLimit=10
>syncLimit=5
>dataDir=/data/zookeeper/data
>clientPort=2181
>server.1 = hadoop1:2888:3888
>server.2 = hadoop2:2888:3888
>server.3 = hadoop3:2888:3888
>server.4 = hadoop4:2888:3888
>server.5 = hadoop5:2888:3888

服务器标识配置
创建文件/data/zookeeper/data/myid,各节点配置内容为1，2，3，4，5。

 

### 5 HDFS初始化

#### 5.1 启动zk与journalnode

在所有节点执行

```bash
zkServer.sh start
hadoop-daemon.sh start journalnode
jsp
```

#### 5.2 创建ZK命名空间

在主namenode节点执行

```bash
hdfs zkfc -formatZK
```

执行命令：zkCli.sh
查看节点命令：ls /

#### 5.3 格式化namenode和journalnode目录

在主namenode节点执行

```bash
hdfs namenode -format hdfs
```

#### 5.4 启动namenode，同步元数据到备namenode

在主namenode节点执行

```bash
hadoop-daemon.sh start namenode
```

在备namenode节点执行

```bash
hdfs namenode -bootstrapStandby
hadoop-daemon.sh start namenode
```

#### 5.5 启动ZKFC

在主备namenode节点执行

```bash
hadoop-daemon.sh start zkfc
```

#### 5.6 启动datanode

在所有节点执行

```bash
hadoop-daemon.sh start datanode
```

#### 5.7 启动yarn

在主namenode节点执行

```bash
start-yarn.sh
```

在备namenode节点执行

```bash
yarn-daemon.sh start resourcemanager
```

#### 5.8 启动jobhistory

在主namenode节点执行

```bash
mr-jobhistory-daemon.sh start historyserver
```

 

### 6 Hadoop集群状态

#### 6.1 HDFS HA状态

```bash
hdfs haadmin -getServiceState nn1
hdfs haadmin -getServiceState nn2
```

#### 6.2 yarn HA状态

```bash
yarn rmadmin -getServiceState rm1
yarn rmadmin -getServiceState rm2
```

#### 6.3 Hadoop web页面

HDFS管理界面：http://10.206.230.222:50070/

YARN管理界面：http://10.206.230.222:8088/

JobHistory管理界面：http://10.206.230.222:19888/

 

### 7 日常运维

#### 7.1 启停命令

```bash
start-all.sh
stop-all.sh
```

#### 7.2 单模块启动

```bash
hadoop-daemon.sh start journalnode  
hadoop-daemon.sh start namenode
hadoop-daemon.sh start zkfc
hadoop-daemon.sh start datanode
yarn-daemon.sh start resourcemanager
yarn-daemon.sh start nodemanager
mr-jobhistory-daemon.sh start historyserver
```

 

### 8 验证namenode HA

在Hadoop1上传文件hosts文件：hdfs dfs -put /etc/hosts /

杀掉Hadoop1 NameNode进程

查看进程：jps

在Hadoop2查看hosts文件：hdfs dfs -ls /hosts

 

### 9 测试MapReduce Job

**9.1 创建输入HDFS目录**

```bash
hdfs dfs -mkdir -p /wordcountdemo/input
```

**9.2 创建原始文件**

```bash
vi /app/wc.input
```

>hadoop mapreduce hive
>hbase spark storm
>sqoop hadoop hive
>spark hadoop

**9.3 将wc.input文件上传到HDFS的/wordcountdemo/input目录**

```bash
hdfs dfs -put /app/wc.input /wordcountdemo/input
```

**9.4 运行WordCount MapReduce Job**

```shell
# 输出目录不能创建，yarn会自动创建
bin/yarn jar /app/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.0.jar wordcount /wordcountdemo/input /wordcountdemo/output
```

**9.5 查看输出结果目录**

```bash
hdfs dfs -ls /wordcountdemo/output
```

**9.6 查看输出文件内容**

```bash
hdfs dfs -cat /wordcountdemo/output/part-r-00000
```

 