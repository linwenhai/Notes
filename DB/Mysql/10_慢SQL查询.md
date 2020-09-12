## Mysql慢SQL查询



```sql
/* 查看当前慢SQL */
select id,info,time from information_schema.processlist 
where command <> 'sleep' 
and user not in ('event_scheduler','root','repl','tivoli','mgrdba','system user') 
and time>80;
```



