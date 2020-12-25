## Sqoop

### 1 下载sqoop

sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz

 

### 2 sqoop-env.sh文件配置

```bash
vi /app/sqoop/conf/sqoop-env.sh
修改内容：



export HADOOP_COMMON_HOME=/app/hadoop



export HADOOP_MAPRED_HOME=/app/hadoop



export HBASE_HOME=/app/hbase



export HIVE_HOME=/app/hive



export ZOOCFGDIR=/app/zookeeper/conf
```

 

### 3 拷贝mysql驱动

mysql-connector-java-5.1.40-bin.jar

```bash
cp mysql-connector-java-5.1.40-bin.jar /app/sqoop/lib4
```

 

### 4 验证版本

```bash
sqoop-version
```

 

### 5 使用sqoop查看mysql中的数据表

```bash
./sqoop list-databases --connect jdbc:mysql://10.203.197.48:3306/hive?characterEncoding=UTF-8 --username hive --password 'hive'
```

 

### 6 把MySQL中的表导入hdfs中

```bash
sqoop import -m 1 --connect jdbc:mysql://10.203.197.48:3306/hive--username hive --password 'hive' --table name --target-dir /user/sqoop/datatest
```

 

 