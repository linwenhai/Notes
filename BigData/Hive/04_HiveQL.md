## HiveQL

### 1 创建库

```SHELL
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

```shell
# 查看数据库
hive> show databases;
 
# 显示数据库的详细属性信息
hive> desc database extended t3;
 
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



5 创建表









