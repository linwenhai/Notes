## Hadoop伪分布环境搭建

 hadoop版本：hadoop-2.7.7.tar.gz

Jdk版本：jdk-8u261-linux-x64.tar.gz

OS版本：CentOS Linux release 7.8.2003 (Core)

官网下载：http://archive.apache.org/dist/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz

国内镜像下载：https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz



### 1 基础配置

```bash
# 配置hosts文件
192.168.100.11	test
```

 

```bash
# 配置JDK
cd /opt
tar -zxvf jdk-8u261-linux-x64.tar.gz
ln -s jdk1.8.0_261 jdk
vi /etc/profile
```

 >export JAVA_HOME=/opt/jdk
 >export PATH=$JAVA_HOME/bin:$PATH

```bash
source /etc/profile
java -version
```



```bash
# 配置免密SSH登陆
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
ssh localhost
```

 

```bash
tar -zxvf hadoop-2.7.7.tar.gz
ln -s hadoop-2.7.7 hadoop
vi /etc/profile
```



```bash
mkdir -p /opt/hadoop/tmp
mkdir -p /opt/hadoop/data/nn
mkdir -p /opt/hadoop/data/dn
mkdir -p /opt/hadoop/logs
```



### 2 Hadoop配置

1】etc/hadoop/core-site.xml配置文件

```XML
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://test:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/hadoop/tmp</value>
    </property>
</configuration>
```

 2】etc/hadoop/hdfs-site.xml配置文件

```XML
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///opt/hadoop/data/nn</value>
    </property>
        <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///opt/hadoop/data/dn</value>
    </property> 
</configuration>
```

 3】etc/hadoop/mapred-site.xml配置文件

```XML
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

 4】yarn-site.xml配置文件

```XML
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

 5】hadoop-env.sh配置文件

```cs
export JAVA_HOME=/opt/jdk
export HADOOP_LOG_DIR=/opt/hadoop/logs
```

 6】yarn-env.sh配置文件

```cs
export JAVA_HOME=/opt/jdk
export HADOOP_LOG_DIR=/opt/hadoop/logs
```

 

### 3 格式化namenode

```bash
bin/hdfs namenode -format
```

 

### 4 启动HDFS/YARN

```bash
sbin/start-dfs.sh
sbin/start-yarn.sh
```

 

### 5 停止HDFS/YARN

```bash
sbin/stop-dfs.sh
sbin/stop-yarn.sh
```

 

### 6 Hadoop WEB

http://192.168.100.11:50070/
http://192.168.100.11:8088/

 

### 7 测试

```bash
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/test
bin/hdfs dfs -put etc/hadoop input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar grep input output 'dfs[a-z.]+'
bin/hdfs dfs -get output output
cat output/* 
bin/hdfs dfs -cat output/*
```

 