

1 查询Oracle正在执行的sql语句及执行该语句的用户

```sql
SELECT b.sid oracleID,  
       b.username Oracle用户,  
       b.serial#,  
       spid 操作系统ID,  
       paddr,  
       sql_text 正在执行的SQL,  
       b.machine 计算机名  
FROM v$process a, v$session b, v$sqlarea c  
WHERE a.addr = b.paddr  
   AND b.sql_hash_value = c.hash_value;
```



2 查看正在执行sql的发起者的发放程序

```sql
SELECT A.serial#,OSUSER 电脑登录身份,
       PROGRAM 发起请求的程序,  
       USERNAME 登录系统的用户名,  
       SCHEMANAME,  
       B.Cpu_Time 花费cpu的时间,  
       STATUS,  
       B.SQL_TEXT 执行的sql  
FROM V$SESSION A  
LEFT JOIN V$SQL B ON A.SQL_ADDRESS = B.ADDRESS  
                   AND A.SQL_HASH_VALUE = B.HASH_VALUE  
ORDER BY b.cpu_time DESC 
```

