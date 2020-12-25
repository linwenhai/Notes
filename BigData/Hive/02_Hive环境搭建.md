## Hive1.2.2环境搭建

### 1 创建库和用户

```sql
mysql> create user 'hive' identified by 'hive';
mysql> grant all on *.* TO 'hive'@'%' identified by 'hive' with grant option;
mysql> grant all on *.* TO 'hive'@'localhost' identified by 'hive' with grant option;
mysql> flush privileges;
```

```sql
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

```bash
tar -zxvf apache-hive-1.2.2-bin.tar.gz
ln -s apache-hive-1.2.2-bin hive
cp mysql-connector-java-5.1.40-bin.jar /app/hive/lib/
```



### 4 环境变量

```bash
vi /etc/profile
```

>export JAVA_HOME=/app/jdk export JRE_HOME=${JAVA_HOME}/jre 
>export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH 
>export PATH=$JAVA_HOME/bin:$PATH
>
>export HADOOP_HOME=/app/hadoop 
>export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
>
>export HIVE_HOME=/app/hive 
>export PATH=$PATH:$HIVE_HOME/bin

```bash
source /etc/profile
```



### 5 hive初始化

```bash
schematool -dbType mysql -initSchema
```



### 6 创建hive数据库

```bash
hive> create database myhive;
hive> use myhive;
hive> select current_database();    	--查看当前正在使用的数据库
```



### 7 创建表

```bash
hive> create table student(id int, name string, sex string, age int, department string) row format delimited fields terminated by ",";
```



### 8 创建TXT

```bash
vi /app/student.txt
```

```
95002,刘晨,女,19,IS 
95017,王风娟,女,18,IS 
95018,王一,女,19,IS 
95013,冯伟,男,21,CS 
95014,王小丽,女,19,CS 
95019,邢小丽,女,19,IS 
95020,赵钱,男,21,IS 
95003,王敏,女,22,MA 
95004,张立,男,19,IS 
95012,孙花,女,20,CS 
95010,孔小涛,男,19,CS 
95005,刘刚,男,18,MA 
95006,孙庆,男,23,CS 
95007,易思玲,女,19,MA 
95008,李娜,女,18,CS 
95021,周二,男,17,MA 
95022,郑明,男,20,MA 
95001,李勇,男,20,CS 
95011,包小柏,男,18,MA 
95009,梦圆圆,女,18,MA 
95015,王君,男,18,MA
```



### 9 往表中加载数据

```bash
hive> load data local inpath "/app/student.txt" into table student;
hive> select * from student;
```



### 10 查看表结构

```bash
hive> desc student;
hive> desc extended student;
hive> desc formatted student;
```

