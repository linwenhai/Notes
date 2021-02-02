## SQL优化



#### 一、explain

```sql
explain select * from emp3 where store_id='234';
```

![img](image/20210108180700722.png)



【1】select_type：select类型

| SIMPLE   | 简单表               |
| -------- | -------------------- |
| PRIMARY  | 主查询，即外层的查询 |
| UNION    | union中第二个select  |
| SUBQUERY | 子查询第一个select   |

【2】table：输出结果表

【3】partitions：分区

【4】type：访问类型

性能：ALL最差，NULL最好

|      | 访问类型     | 描述                                                    |
| ---- | ------------ | ------------------------------------------------------- |
| 1    | ALL          | 全部扫描                                                |
| 2    | index        | 索引全扫描，遍历整个索引                                |
| 3    | range        | 索引范围扫描，用于<、<=，>，>=，betweem                 |
| 4    | ref          | 使用非唯一性索引或唯一性索引的前缀扫描                  |
| 5    | eq_ref       | 唯一性索引                                              |
| 6    | const,system | 单表中最多有一匹配行，如根据primary keyey或唯一索引查询 |
| 7    | NULL         | mysql不用访问表或索引                                   |

【5】possible_keys：表示查询时可能使用的索引

【6】key：表示实际使用的索引

【7】key_len：使用索引的长度

【8】rows：扫描行数量

【9】filtered：

【10】Extra：执行情况的说明



#### 二、profile

```sql
-- profile默认关闭
mysql> select @@profiling;
+-------------+
| @@profiling |
+-------------+
|           0 |
+-------------+
 
-- 开启
mysql> set profiling=1;
```



```sql
mysql> select * from emp3;
+------+-------+------------+------------+-------+----------+
| id   | ename | hired      | separated  | job   | store_id |
+------+-------+------------+------------+-------+----------+
| 1001 | lin   | 2015-01-01 | 9999-12-31 | clerk |      234 |
+------+-------+------------+------------+-------+----------+
 
mysql> show profiles;
+----------+------------+--------------------+
| Query_ID | Duration   | Query              |
+----------+------------+--------------------+
|        1 | 0.00108600 | show databases     |
|        2 | 0.00048550 | SELECT DATABASE()  |
|        3 | 0.00040500 | show databases     |
|        4 | 0.00037225 | show tables        |
|        5 | 0.00057425 | select * from emp3 |
+----------+------------+--------------------+

-- 主要查看：Sending data
mysql> show profile for query 5;
+----------------------+----------+
| Status               | Duration |
+----------------------+----------+
| starting             | 0.000093 |
| checking permissions | 0.000008 |
| Opening tables       | 0.000020 |
| init                 | 0.000033 |
| System lock          | 0.000013 |
| optimizing           | 0.000005 |
| statistics           | 0.000019 |
| preparing            | 0.000010 |
| executing            | 0.000003 |
| Sending data         | 0.000317 |
| end                  | 0.000008 |
| query end            | 0.000008 |
| closing tables       | 0.000008 |
| freeing items        | 0.000013 |
| cleaning up          | 0.000016 |
+----------------------+----------+
 
-- 查看CPU消耗
mysql> show profile cpu for query 5;
+----------------------+----------+----------+------------+
| Status               | Duration | CPU_user | CPU_system |
+----------------------+----------+----------+------------+
| starting             | 0.000093 | 0.000044 |   0.000044 |
| checking permissions | 0.000008 | 0.000003 |   0.000004 |
| Opening tables       | 0.000020 | 0.000010 |   0.000010 |
| init                 | 0.000033 | 0.000017 |   0.000017 |
| System lock          | 0.000013 | 0.000006 |   0.000006 |
| optimizing           | 0.000005 | 0.000003 |   0.000003 |
| statistics           | 0.000019 | 0.000009 |   0.000009 |
| preparing            | 0.000010 | 0.000005 |   0.000005 |
| executing            | 0.000003 | 0.000002 |   0.000002 |
| Sending data         | 0.000317 | 0.000158 |   0.000160 |
| end                  | 0.000008 | 0.000003 |   0.000003 |
| query end            | 0.000008 | 0.000004 |   0.000004 |
| closing tables       | 0.000008 | 0.000004 |   0.000004 |
| freeing items        | 0.000013 | 0.000006 |   0.000007 |
| cleaning up          | 0.000016 | 0.000008 |   0.000008 |
+----------------------+----------+----------+------------+
 
-- 查看io消耗
mysql> mysql> show profile block io for query 5;
+----------------------+----------+--------------+---------------+
| Status               | Duration | Block_ops_in | Block_ops_out |
+----------------------+----------+--------------+---------------+
| starting             | 0.000093 |            0 |             0 |
| checking permissions | 0.000008 |            0 |             0 |
| Opening tables       | 0.000020 |            0 |             0 |
| init                 | 0.000033 |            0 |             0 |
| System lock          | 0.000013 |            0 |             0 |
| optimizing           | 0.000005 |            0 |             0 |
| statistics           | 0.000019 |            0 |             0 |
| preparing            | 0.000010 |            0 |             0 |
| executing            | 0.000003 |            0 |             0 |
| Sending data         | 0.000317 |            0 |             0 |
| end                  | 0.000008 |            0 |             0 |
| query end            | 0.000008 |            0 |             0 |
| closing tables       | 0.000008 |            0 |             0 |
| freeing items        | 0.000013 |            0 |             0 |
| cleaning up          | 0.000016 |            0 |             0 |
+----------------------+----------+--------------+---------------+
```





#### 三、慢SQL

```sql
mysql> show processlist;
```



```sql
-- 查询非 Sleep 状态的链接，按消耗时间倒序展示，自己加条件过滤
select id, db, user, host, command, time, state, info
from information_schema.processlist
where command != 'Sleep'
and time>120
order by time desc;
```

- **Id**：链接mysql 服务器线程的唯一标识，可以通过kill来终止此线程的链接。
- **User**：当前线程链接数据库的用户
- **Host**：显示这个语句是从哪个ip 的哪个端口上发出的。可用来追踪出问题语句的用户
- **db**: 线程链接的数据库，如果没有则为null
- **Command**: 显示当前连接的执行的命令，一般就是休眠或空闲（sleep），查询（query），连接（connect）
- **Time**: 线程处在当前状态的时间，单位是秒
- **State**：显示使用当前连接的sql语句的状态，很重要的列，后续会有所有的状态的描述，请注意，state只是语句执行中的某一个状态，一个 sql语句，已查询为例，可能需要经过copying to tmp table，Sorting result，Sending data等状态才可以完成
- **Info**: 线程执行的sql语句，如果没有语句执行则为null。这个语句可以使客户端发来的执行语句也可以是内部执行的语句





#### 四、连接数

```sql
mysql> show status like 'Threads%';      -- 查看当前实时连接数
+-------------------+-------+  
| Variable_name     | Value |  
+-------------------+-------+  
| Threads_cached    | 58    |  
| Threads_connected | 57    |   -- 打开的连接数  
| Threads_created   | 3676  |  
| Threads_running   | 4     |   -- 激活的连接数，这个数值一般远低于connected数值  
+-------------------+-------+  
```



```sql
mysql> show variables like '%max_connections%';   -- 最大连接数
+-----------------+-------+  
| Variable_name   | Value |  
+-----------------+-------+  
| max_connections | 100  |  
+-----------------+-------+ 
```

```sql
-- 可以在/etc/my.cnf里面设置数据库的最大连接数
max_connections = 1000
```



```java
jdbc.initialSize=0 //初始化连接数
 
jdbc.maxActive=30 //最大数据库连接数，0表示无限制
 
jdbc.maxIdle=20 //最大闲置的连接个数，0表示没有限制
 
jdbc.maxWait=1000 //超时等待时间，以毫秒为单位
 
jdbc.removeAbandoned=true //是否自动回收超时连接
 
jdbc.removeAbandonedTimeout=60 //设置被遗弃的连接的超时的时间，以秒数为单位
 
jdbc.logAbandoned = true //是否在自动回收超时连接的时候打印连接的超时错误
 
jdbc.validationQuery=select 1 from dual //给出一条简单的sql语句进行验证
 
jdbc.testOnBorrow=true //在取出连接时进行有效验证
```



