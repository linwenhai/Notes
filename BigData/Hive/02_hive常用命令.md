

### 1 shell执行hive命令

```shell
hive -e "select * from test01 limit 3";
```



### 2 从文件中执行hive查询

```
hive> source /app/01/01.sql;
```



### 3 执行shell命令

```
hive> ! pwd;
```



### 4 知道Hadoop的dfs命令

```
hive> dfs -ls / ;
```



### 5 hive注释

以--开头的字符串表示注释。

