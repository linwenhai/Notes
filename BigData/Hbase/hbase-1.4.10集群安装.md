## Hbase分布式环境搭建



### 1 环境需求

JDK1.8

zookeeper

hadoop



### 2 安装hbase

```shenll
tar -zxvf hbase-1.4.10-bin.tar.gz
ln -s hbase-1.4.10 hbase
```



### 3 配置hbase环境变量







### 4 配置hbase-env.sh文件

```shell
#关闭自带的ZK
export HBASE_MANAGES_ZK=false

#配置hbase读取hdfs配置
export HBASE_CLASSPATH=/app/hadoop/etc/hadoop/

#设置hbase日志目录
export HBASE_LOG_DIR=/logs/hbase
```



### 5 配置hbase-site.xml文件

```xml
<!-- 数据存储目录 -->
<property> 
	<name>hbase.rootdir</name>
	<value>hdfs://mycluster/hbase</value>
</property> 

<!-- 是否分布式部署 -->
<property> 
	<name>hbase.cluster.distributed</name>
	<value>true</value> 
</property>

<!-- zk地址 -->
<property>
	<name>hbase.zookeeper.quorum</name>
	<value>nn01,nn02,dn01,dn02,dn03</value>
</property>
```



### 6 启动hbase

```shell
#启动master
bin/hbase-daemon.sh start master
jps

#启动regionserver
bin/hbase-daemon.sh start regionserver
jps
```







