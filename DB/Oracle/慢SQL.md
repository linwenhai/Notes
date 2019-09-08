

#### 1 查看总消耗时间最多的前10条SQL语句

```sql
select *
from (select v.sql_id,
v.child_number,
v.sql_text,
last_load_time,
v.PARSING_USER_ID,
ROUND(v.ELAPSED_TIME / 1000000 / (CASE
               WHEN (EXECUTIONS = 0 OR NVL(EXECUTIONS, 1 ) = 1) THEN
                1
               ELSE
                EXECUTIONS
             END),
             2) "执行时间'S'",
v.SQL_FULLTEXT,
v.cpu_time,
v.disk_reads,
rank() over(order by v.elapsed_time desc) elapsed_rank
from v$sql v  ) a
where elapsed_rank <= 100  
and last_load_time > to_char(sysdate - 1/24, 'YYYY-MM-DD/HH:MI:SS')    
order by "执行时间'S'" desc
```



#### 2 查询最近一小时内最慢的SQL

```sql
select executions, cpu_time/1e6 as cpu_sec, elapsed_time/1e6 as elapsed_sec, round(elapsed_time/sqrt(executions)) as important, v.* 
from v$sql v 
where executions > 10 and last_load_time > to_char(sysdate - 1/24, 'YYYY-MM-DD/HH:MI:SS')  
order by important desc
```



#### 3 查看CPU消耗时间最多的前10条SQL语句

```sql
select *
from (select v.sql_id,
v.child_number,
v.sql_text,
v.elapsed_time,
v.cpu_time,
v.disk_reads,
rank() over(order by v.cpu_time desc) elapsed_rank
from v$sql v) a
where elapsed_rank <= 10;
```



#### 4 查看消耗磁盘读取最多的前10条SQL语句

```sql
select *
from (select v.sql_id,
v.child_number,
v.sql_text,
v.elapsed_time,
v.cpu_time,
v.disk_reads,
rank() over(order by v.disk_reads desc) elapsed_rank
from v$sql v) a
where elapsed_rank <= 10;
```



#### 5 查询执行最慢的sql

```sql
select *
 from(selectsa.SQL_TEXT,
        sa.SQL_FULLTEXT,
        sa.EXECUTIONS"执行次数",
        round(sa.ELAPSED_TIME / 1000000, 2)"总执行时间",
        round(sa.ELAPSED_TIME / 1000000 / sa.EXECUTIONS, 2)"平均执行时间",
        sa.COMMAND_TYPE,
        sa.PARSING_USER_ID"用户ID",
        u.username"用户名",
        sa.HASH_VALUE
     fromv$sqlarea sa
     leftjoinall_users u
      onsa.PARSING_USER_ID = u.user_id
     wheresa.EXECUTIONS > 0
     orderby(sa.ELAPSED_TIME / sa.EXECUTIONS)desc)
 whererownum <= 50;
```



#### 6 查询执行次数最多的 SQL

```sql
select *
 from(selects.SQL_TEXT,
        s.EXECUTIONS"执行次数",
        s.PARSING_USER_ID"用户名",
        rank() over(orderbyEXECUTIONS desc) EXEC_RANK
     fromv$sql s
     leftjoinall_users u
      onu.USER_ID = s.PARSING_USER_ID) t
 whereexec_rank <= 100;
```

