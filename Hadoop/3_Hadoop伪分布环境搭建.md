## Hadoop伪分布环境搭建

 

### 1 hosts

```
192.168.1.100    test
```

 

### 2 JDK

```bash
cd /opt
tar -zxvf jdk-8u201-linux-x64.tar.gz
ln -s jdk-8u201-linux-x64 jdk
vi /etc/profile
```

 >export JAVA_HOME=/opt/jdk
 >export PATH=$JAVA_HOME/bin:$PATH



### 3 免密SSH

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
ssh localhost
```

 

### 4 core-site.xml配置文件

```XML
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://test:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/data/hadoop/tmp</value>
    </property>    
</configuration>
```

 

### 5 hdfs-site.xml配置文件

```XML
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
        <value>file:///data/hadoop/hdfs//dn</value>
    </property> 
</configuration>
```

 

### 6 mapred-site.xml配置文件

```XML
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

 

### 7 yarn-site.xml配置文件

```XML
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

 

### 8 hadoop-env.sh配置文件

```cs
export JAVA_HOME=/opt/jdk
export HADOOP_LOG_DIR=/logs/hadoop
```

 

### 9 yarn-env.sh配置文件

```cs
export JAVA_HOME=/opt/jdk
export HADOOP_LOG_DIR=/logs/hadoop
```

 

### 10 创建目录

```bash
mkdir -p /logs/hadoop 
mkdir -p /data/hadoop/tmp
mkdir -p /data/hadoop/hdfs/nn
mkdir -p /data/hadoop/hdfs/dn
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

 

### 14 Hadoop WEB

http://localhost:50070/
http://localhost:8088/

 

### 15 测试

```bash
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/ceshi
bin/hdfs dfs -put etc/hadoop input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.5.jar grep input output 'dfs[a-z.]+'
bin/hdfs dfs -get output output
cat output/* 
bin/hdfs dfs -cat output/*
```

 