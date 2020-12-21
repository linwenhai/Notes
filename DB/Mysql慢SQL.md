## Mysql慢SQL





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



