

#### 1 会话

```sql
-- 查看当前session
select username,count(username) from v$session where username is not null group by username;
 
-- 当前连接数
select count(*) from v$process;
 
-- 并发连接数
Select count(*) from v$session where status='ACTIVE';
 
 
-- 查询数据库实例启动以来最大连接数
select resource_name,MAX_UTILIZATION,LIMIT_VALUE from v$resource_limit
where resource_name in ('processes','sessions');
```

```sql
-- 查看参数设置，sessions=(1.1*processes)+5
select name,value from v$parameter where name = 'processes';
```



#### 2 死锁

- Username：死锁语句所用的数据库用户。
- Lockwait：死锁的状态，如果有内容表示被死锁。
- Machine： 死锁语句所在的机器。

```sql
select sess.sid,
       sess.serial#,
       vp.SPID,
       lo.oracle_username,
       sess.lockwait,
       ao.object_name,
       lo.locked_mode,
       sess.machine,
       'alter system kill session  '||''''||sess.SID||','||sess.SERIAL#||''''||';'
  from v$locked_object lo, dba_objects ao, v$session sess, v$process vp
where ao.object_id = lo.object_id
   and lo.session_id = sess.sid
   and sess.PADDR = vp.ADDR;
```



#### 3 慢SQL

##### 3.1 查询历史

1】查询快照开始和结束时间

```sql
select snap_id,END_INTERVAL_TIME 
from dba_hist_snapshot a
where a.END_INTERVAL_TIME >= to_date('2020-11-22','YYYY-MM-DD')
and a.END_INTERVAL_TIME < to_date('2020-11-23','YYYY-MM-DD')
order by snap_id desc
```



2】SQL ordered by user i/o wait time

```sql
select *
  from (select round(nvl((sqt.iowait / 1000000), to_number(null)), 2) "USER I/O Time (s)",
               round(nvl((sqt.elap / 1000000), to_number(null)), 2) "Elap Time (s)",
               sqt.exec,
               round(decode(sqt.exec,
                            0,
                            to_number(null),
                            (sqt.iowait / sqt.exec / 1000000)),
                     2) "UIO per Exec (s)",
               round((100 * (sqt.elap /
                     (select sum(e.value) - sum(b.value)
                                from dba_hist_sys_time_model b,
                                     dba_hist_sys_time_model e
                               where b.snap_id = &beg_snap
                                 and e.snap_id = &end_snap
                                 and e.stat_name = 'DB time'
                                 and b.stat_name = 'DB time'))),
                     2) norm_val,
               sqt.sql_id,
               decode(sqt.module, null, null, 'Module: ' || sqt.module) sqlModule,
               nvl(to_nchar(SUBSTR(st.sql_text, 1, 2000)),
                   (' ** SQL Text Not Available ** ')) SqlText
          from (select sql_id,
                       max(module) module,
                       sum(iowait_delta) iowait,
                       sum(cpu_time_delta) cput,
                       sum(elapsed_time_delta) elap,
                       sum(executions_delta) exec
                  from dba_hist_sqlstat
                 where &beg_snap < snap_id
                   and snap_id <= &end_snap
                 group by sql_id) sqt,
               dba_hist_sqltext st
         where st.sql_id(+) = sqt.sql_id
         order by nvl(sqt.iowait, -1) desc, sqt.sql_id)
 where rownum < 65
   and (rownum <= 10 or norm_val > 1);
```



3】SQL ordered by cpu time

```bash
select *
  from (select round(nvl((sqt.cput / 1000000), to_number(null)), 2) "CPU Time (s)",
               round(nvl((sqt.elap / 1000000), to_number(null)), 2) "Elap Time (s)",
               sqt.exec,
               round(decode(sqt.exec,
                            0,
                            to_number(null),
                            (sqt.cput / sqt.exec / 1000000)),
                     2) "CPU per Exec (s)",
               round((100 * (sqt.elap /
                     (select sum(e.value) - sum(b.value)
                                from dba_hist_sys_time_model b,
                                     dba_hist_sys_time_model e
                               where b.snap_id = &beg_snap
                                 and e.snap_id = &end_snap
                                 and e.stat_name = 'DB time'
                                 and b.stat_name = 'DB time'))),
                     2) norm_val,
               sqt.sql_id,
               decode(sqt.module, null, null, 'Module: ' || sqt.module) sqlModule,
               nvl(to_nchar(SUBSTR(st.sql_text, 1, 2000)),
                   (' ** SQL Text Not Available ** ')) SqlText
          from (select sql_id,
                       max(module) module,
                       sum(cpu_time_delta) cput,
                       sum(elapsed_time_delta) elap,
                       sum(executions_delta) exec
                  from dba_hist_sqlstat
                 where &beg_snap < snap_id
                   and snap_id <= &end_snap
                 group by sql_id) sqt,
               dba_hist_sqltext st
         where st.sql_id(+) = sqt.sql_id
         order by nvl(sqt.cput, -1) desc, sqt.sql_id)
 where rownum < 65
   and (rownum <= 10 or norm_val > 1);
```



4】根据ID查询SQL

```sql
select SQL_TEXT,SQL_FULLTEXT
from  v$sql where sql_id IN ('acmhs5f60v3zb','0w40uu79ntfc2');
```



##### 3.2 实时查询

先通过top命令查看产用资源较多的spid号。

```sql
SELECT sql_text
  FROM v$sqltext a
WHERE (a.hash_value, a.address) IN
       (SELECT DECODE(sql_hash_value, 0, prev_hash_value, sql_hash_value),
               DECODE(sql_hash_value, 0, prev_sql_addr, sql_address)
          FROM v$session b
         WHERE b.paddr = (SELECT addr FROM v$process c WHERE c.spid = '&pid'))
ORDER BY piece ASC
```





