## Hive SQL

### 1 创建/删除库

```bash
# 创建新库
hive> create database test;
hive> show databases;
```

```bash
# 删除不含表的数据库
hive> drop database if exists test;

# 删除含有表的数据库
hive> drop database if exists test cascade;
```

```bash
hive> use test;
hive> select current_database();	#查看正在使用哪个库
```



### 2 创建表

参数：

- create table ：创建一个指定名字的表
- if not exist ：如表存在，忽略报错
- external ：创建一个外部表，需指定一个指向实际数据的路径
- partitioned by ：创建分区
- clustered by ：创建桶
- fields terminated by ',' ：字段以逗号分隔
- fields terminated by '\001' ：字段以制表符分隔



#### 2.1 内部表

```bash
hive>
create table s1(id int,name string) 
row format delimited 
fields terminated by '\t';
```

```bash
# 查看表结构
hive> desc s1;
hive> desc formatted s1;
```



#### 2.2 外部表

```bash
hive>
create external table e1(id int,name string) 
row format delimited 
fields terminated by '\t'
location 'hdfs://test:9000/hive/warehouse/test.db/s1';
```



#### 2.3 创建分区表

```bash
# 创建单分区表
hive>
create table p1(name string,school string) 
partitioned by (ctime string) 
row format delimited 
fields terminated by '\t';
```

```bash
# 创建多分区表,多个分区字段时，是有先后顺序的
hive> 
create table p2(name string,school string) 
partitioned by (id int,address string) 
row format delimited 
fields terminated by '\t';
```



### 3 删除表

```bash
# 删除表
hive> 
drop table s1;
```



### 4 修改表

```bash
# 添加分区
hive> 
alter table p1 add partition(ctime='20201001');
alter table p1 add partition(ctime='20201002');
```

```bash
# 查看分区
hive> 
show partitions p1;
```



