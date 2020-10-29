## Hive1.2.2环境搭建

![yo2sgi0ktk](D:\Notes\BigData\Hive\image\yo2sgi0ktk.png)

下载：

https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-1.2.2/apache-hive-1.2.2-bin.tar.gz



### 1 创建mysql库和用户

```sql
mysql> create user 'hive' identified by 'hive';
mysql> grant all on *.* TO 'hive'@'%' identified by 'hive' with grant option;
mysql> grant all on *.* TO 'hive'@'localhost' identified by 'hive' with grant option;
mysql> flush privileges;
```

```sql
bin/mysql -uhive -phive
mysql> create database hive;
mysql> show databases;
```



### 2 配置hive

1】conf/hive-site.xml配置文件

```xml
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://192.168.100.11:3306/hive?createDatabaseIfNotExist=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;useSSL=false</value>
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

2】MySQL驱动包

```bash
tar -zxvf apache-hive-1.2.2-bin.tar.gz
cp mysql-connector-java-5.1.40-bin.jar /opt/hive/lib/
```

3】环境变量

```bash
vi /etc/profile
```

>export HIVE_HOME=/opt/hive 
>export PATH=$PATH:$HIVE_HOME/bin

```bash
source /etc/profile
```

4】hive初始化

```bash
bin/schematool -dbType mysql -initSchema
```



### 3 运行hive

```bash
bin/hive
```



### 4 测试

```bash
# 创建myhive
hive> create database myhive;
```

```bash
# 创建表student
hive> use myhive;
hive> create table student(id int, name string, sex string, age int, department string) row format delimited fields terminated by ",";
```

```bash
# 创建TXT
vi /tmp/student.txt
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

```bash
# 往表中加载数据
hive> load data local inpath "/tmp/student.txt" into table student;
hive> select * from student;
```



