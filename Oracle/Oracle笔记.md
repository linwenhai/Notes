**Oracle笔记**



## 1 Oracle网址

1] my support
https://support.oracle.com/epmos/faces/MosIndex.jspx?_afrLoop=772106189111605&_afrWindowMode=0&_adf.ctrl-state=13lqxgk6ej_245

2] documentation library
http://www.oracle.com/pls/db112/homepage

3] oracle-base
http://www.oracle-base.com/index.php

4] 11.2.0.3安装包
https://support.oracle.com/epmos/faces/ui/patch/PatchDetail.jspx?_afrLoop=181284479270492&patchId=10404530&languageId=en&_afrWindowMode=0&_adf.ctrl-state=12kngzo3f0_321



## 2 体系结构

>DDL:  Data Definition Language (数据定义语言)， Contain：Create, Alter, Drop 
>DML:  Data Manipulation Language (数据操作语言)， Contain：Select, Update, Insert, Delete
>DCL:  Data Control Language (是数据库控制语言) ，Contain：Grant, Deny, Revoke



>Oracle有个比列80%*memory。OLTP系统，其中sga80%，pga20%。DDS系统，各50%。你可以给小点。运行一段时间后，查看v$pga_target_advice和v$sga_target_advice来看Oracle给出的建议。



### 2.1 SGA

SGA: system global area(系统全局区),是一块用于加载数据、对象并保存运行状态和数据库控制信息的一块。

```sql
--查看SGA粒度大小
--调整参数值是粒度的整数倍
select component,granule_size/1024/1024 from v$sga_dynamic_components;
```

>Linux/Unix系统建议shmmax比sga参数设置大



Buffer Cache(缓冲区高速缓存)： 用于存储最近使用的数据块。

```
缓冲池参数：
	. db_cache_size
	. db_keep_cache_size
	. db_recycle_cache_size
```



```sql
--db_cache_size修改参考
select id,name,block_size,size_for_estimate sfe, size_factor sf,estd_physical_read_factor eprf,estd_physical_reads epr
from v$db_cache_advice;
```

```sql
--Buffer cache通过链表进行内存管理：LRU List和Dirty List
--Server扫描LRU，将发现修改过的buffer注册到Dirty list，Dirty Queue值超过25%，触发DBWn写操作；
select kvittag,kvitval,kvitdsc from x$kvit where kvittag='kcbldq';

--Server扫描LRU超过40%未找到足够free buffer ，触发DBWn写操作；
select kvittag,kvitval,kvitdsc from x$kvitwhere kvittag='kcbfsp';
```



共享池参数：shared_pool_size

