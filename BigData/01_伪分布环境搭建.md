## 01_伪分布环境搭建

### 1 hosts配置

```bash
vi /etc/hosts
```

```
------添加如下内容----------
192.168.32.100	ceshi
```





### 2 JDK配置

```bash
cd /app
tar -zxvf jdk-8u201-linux-x64.tar.gz
ln -s dk-8u201-linux-x64 jdk
vi /etc/profile
```

```
--------添加如下内容------------
export JAVA_HOME=/app/jdk
export PATH=$JAVA_HOME/bin:$PATH
```

```bash
source /etc/profile
java -version
```



### 3 SSH免密配置

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
ssh localhost
```



### 5 Hadoop安装

```shell
cd /app
tar -zxvf hadoop-2.8.5.tar.gz
ln -s hadoop-2.8.5 hadoop
```



```shell
vi /etc/profile
```

```
------添加如下内容---------------
export HADOOP_HOME=/app/hadoop
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```

```shell
source /etc/profile
```



### 4 core-site.xml文件

```shell
vi $HADOOP_HOME/etc/hadoop/core-site.xml
```

```xml
<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://ceshi:9000</value>
	</property>
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/data/hadoop/tmp</value>
	</property>
</configuration>
```



### 5 hdfs-site.xml文件

```shell
vi $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

```xml
<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
	<property>
		<name>dfs.namenode.name.dir</name>
		<value>file:///data/hadoop/hdfs/nn</value>
	</property>
	<property>
		<name>dfs.datanode.data.dir</name>
		<value>file:///data/hadoop/hdfs/dn</value>
	</property>
</configuration>
```



### 6 mapred-site.xml文件

```shell
vi $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

```xml
<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
</configuration>
```



### 7 yarn-site.xml文件

```shell
vi $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

```xml
<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
</configuration>
```



### 8 hadoop-env.sh

```shell
vi $HADOOP_HOME/etc/hadoop-env.sh
```

```
------添加内容---------
export JAVA_HOME=/app/jdk
export HADOOP_LOG_DIR=/logs/hadoop
```



### 9 yarn-env.sh

```shell
vi $HADOOP_HOME/etc/yarn-env.sh
```

```
-------添加内容--------
export JAVA_HOME=/app/jdk
export YARN_LOG_DIR=/logs/hadoop
```



### 10 创建目录

```bash
mkdir -p /logs/hadoop
mkdir -p /data/hadoop/tmp
mkdir -p /data/hadoop/hdfs/nn
mkdir -p /data/hadoop/hdfs/dn
chown -R ceshi:ceshi /data
chown -R ceshi:ceshi /logs
```



### 11 格式化namenode

```bash
hdfs namenode -format
```



### 12 启动HDFS/YARN

```bash
sbin/start-dfs.sh
sbin/start-yarn.sh
```



### 13 停止HDFS/YARN

```bash
sbin/stop-dfs.sh
sbin/stop-yarn.sh
```



### 14 WEB

http://localhost:50070/
http://localhost:8088/



### 15 测试

```shell
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/ceshi
cd /app/hadoop
hdfs dfs -put etc/hadoop input
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar grep input output 'dfs[a-z.]+'
hdfs dfs -get output output
cat output/* 
hdfs dfs -cat output/*
```



