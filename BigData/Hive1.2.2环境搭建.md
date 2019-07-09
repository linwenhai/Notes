## Hive1.2.2环境搭建

### 1 安装mysql，创建hive用户

```mysql
mysql> create user 'hive' identified by 'hive';
mysql> grant all on *.* TO 'hive'@'%' identified by 'hive' with grant option;
mysql> grant all on *.* TO 'hive'@'localhost' identified by 'hive' with grant option;
mysql> flush privileges;

mysql -uhive -phive
mysql> create database hive;
mysql> show databases;
```



### 2 hive-site.xml配置文件

```xml
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://10.203.197.48:3306/hive?createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;useSSL=false</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>hive</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>hive</value>
    </property>
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/hive/warehouse</value>
    </property>
</configuration>

```



### 3 MySQL驱动包

```shell
tar -zxvf apache-hive-1.2.2-bin.tar.gz
ln -s apache-hive-1.2.2-bin hive
cp mysql-connector-java-5.1.40-bin.jar /app/hive/lib/
```



### 4 环境变量

JDK，HADOOP，HIVE

```shell
vi /etc/profile
```

```
# 添加环境变量
export JAVA_HOME=/app/jdk export JRE_HOME=${JAVA_HOME}/jre 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH 
export PATH=$JAVA_HOME/bin:$PATH

export HADOOP_HOME=/app/hadoop 
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

export HIVE_HOME=/app/hive 
export PATH=$PATH:$HIVE_HOME/bin
```









