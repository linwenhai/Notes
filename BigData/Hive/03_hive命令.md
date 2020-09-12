## Hive命令

### 1 创建库

```bash
# 创建新库
hive> create database t1;

# 创建库的时候检查存与否，如存在不创建
hive> create database if not exists t1;

# 创建库的时候带注释
hive> create database if not exists t2 comment 'leaning hive';

# 创建带属性的库
hive> create database if not exists t3 with dbproperties('creator'='appdeploy','date'='20190625');
```

 

### 2 查看库

```bash
# 查看数据库
hive> show databases;

# 显示数据库的详细属性信息
hive> desc database extended t3;

# 查看正在使用哪个库
hive> select current_database();
```

 

### 3 删除库

```bash
# 删除不含表的数据库
hive> drop database if exists t3;

# 删除含有表的数据库
hive> drop database if exists t2 cascade;
```

### 4 切换库

```bash
hive> use test;
```

 

### 5 创建表

1】创建内部表

```bash
# 创建默认的内部表
hive> 
create table s1(id int,name string,sex string,age int,department string) 
row format delimited fields terminated by ",";
```

 

2】创建外部表

```bash
在建表的同时指定一个指向实际数据的路径（LOCATION）
hive> 
create external table s1_ext(id int,name string,sex string,age int,department string) 
row format delimited fields terminated by "," 
location "hdfs://hdfs/hive/warehouse/test.db/s1";
```



3】创建分区表

```bash
hive> 
create table s1_ptn(id int,name string,sex string,age int,department string)
partitioned by (city string)
row format delimited fields terminated by ",";

# 添加分区
hive> alter table s1_ptn add partition(city="beijing");
hive> alter table s1_ptn add partition(city="shenzhen");
```

如果某张表是分区表，那么每个分区的定义，其实就表现为了这张表的数据存储目录下的一个子目录
如果是分区表，那么数据文件一定要存储在某个分区中，而不能直接存储在表中。