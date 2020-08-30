## Oracle 慢SQL查询



### 1 查询快照开始和结束时间

```sql
select * from (select snap_id,END_INTERVAL_TIME from dba_hist_snapshot order by snap_id desc) 
where rownum<=20;
```



### 2 SQL ordered by user i/o wait time

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



### 3 SQL ordered by cpu time

```sql
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



### 4 根据ID查询SQL

```sql
select SQL_TEXT,SQL_FULLTEXT
from  v$sql where sql_id IN ('acmhs5f60v3zb','0w40uu79ntfc2');
```

