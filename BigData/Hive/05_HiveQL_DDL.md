## HiveQL

### 1 创建库

数据库目录以.db结尾。

```SHELL
# 创建库
hive> create database finabc;
 
# 检查存在与否，如存在不创建
hive> create database if not exists finabc;

# 指定库存储目录
hive> create database t4 location '/tmp/t4.db';

# 增加库描述信息
hive> create database t3 comment 'test db';

```



### 2 查看库

```shell
# 查看所有数据库
hive> show databases;
 
# 显示数据库的详细属性信息
hive> desc database t3;
t3	test db	hdfs://hdfs/hive/warehouse/t3.db	appdeploy	USER
 
# 查看正在使用哪个库
hive> select current_database();
```



### 3 删除库

```shell
# 删除不含表的数据库
hive> drop database if exists t3;
 
# 删除含有表的数据库
hive> drop database if exists t2 cascade;
```



### 4 切换库

```shell
hive> use test;
```



### 5 创建表









