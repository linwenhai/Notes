## SQL基础

### 1 SQL分类

DDL(Data Definition Languages)：数据定义语句，常用关键字create, drop, alter。

DML(Data Manipulation Language)：数据操纵语句，常用关键字insert, delete, update, select。

DCL(Data Control Language)：数据控制语句，常用关键字grant, revoke。



### 2 DDL语句

#### 2】查看所有数据库

```sql
show databases;
```

#### 2】创建/删除数据库

```sql
create database test1;
drop database test1;
```

#### 3】进入数据库

```sql
use test1;
```

#### 4】创建/删除表

```sql
create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(10));
drop table emp;
```

#### 5】查看表定义

```sql
desc emp;
show create table emp \G;
```

#### 6】修改字段类型

```sql
alter table emp modify ename varchar(20);
```

#### 7】增加字段

```sql
alter table emp add column age int(3);
```

#### 8】删除字段

```sql
alter table emp drop column age;
```

#### 9】修改字段名

```sql
alter table emp change age age1 int(4);
```

#### 10】增加字段，指定位置

```sql
alter table emp add birth date after ename;
alter table emp modify age1 int(5) first;
```

#### 11】修改表名

```sql
alter table emp rename emp1;
```





### 3 DML语句

#### 1】insert

```sql
insert into emp(ename,hiredate,sal,deptno) value('lin','1985-04-19',20000,1);
insert into emp(ename,hiredate,sal,deptno) value('xin','1988-06-29',10000,2);
insert into emp(ename,hiredate,sal,deptno) value('zhao','1989-01-23',12000,3);
```

#### 2】update

```sql
update emp set sal=50000 where ename='lin';
```

#### 3】delete

```sql
delete from emp where ename='zhao';
```

#### 4】select

```sql
select * from emp where deptno=1;
```

#### 5】order by

```sql
---默认是升序，desc表示降序
select * from emp order by sal;
select * from emp order by sal desc;
```

#### 6】limit

```sql
---限制分页
select * from emp order by sal limit 1;
---限制分页limit,从第2条记录开始的3条记录
select * from emp order by sal limit 1,3;
```

#### 7】聚合sum, count, max, min, group by, having

```sql
---区别：where是对聚合前过滤，having是对聚合后过滤
select count(1) from emp;
select deptno,count(1) from emp group by deptno;
select deptno,count(1) from emp group by deptno having count(1)>10;
select sum(sal),max(sal),min(sal) from emp;
```

#### 8】内连接

```sql
select ename,deptname from emp,dept where emp.deptno=dept.deptno;
```

#### 9】外连接

```sql
---左连接，包含左边表所有内容和右边表没有与其匹配的内容
---右连接，包含右边表所有内容和左边表没有与其匹配的内容
select ename,deptname from emp left join dept on emp.deptno=dept.deptno;
select ename,deptname from emp right join dept on emp.deptno=dept.deptno;
```

#### 10】子查询

```sql
---表连接性能优于子查询
mysql> select * from emp where deptno in(select deptno from dept);
```

#### 11】记录联合union, union all

```sql

---区别：union直接汇合，union all汇合后distinct
mysql> 	select deptno from emp
		union all
		select deptno from dept;
```





### 4  DCL语句

```sql
---grant授权
grant select on test1.* to 'lin'@'localhost' identified by '123456';

---revoke回收权限
revoke select on test1.* from 'lin'@'localhost';
```

