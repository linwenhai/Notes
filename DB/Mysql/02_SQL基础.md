## SQL基础

### 1 SQL分类

DDL(Data Definition Languages)：数据定义语句，常用关键字create, drop, alter。

DML(Data Manipulation Language)：数据操纵语句，常用关键字insert, delete, update, select。

DCL(Data Control Language)：数据控制语句，常用关键字grant, revoke。



### 2 DDL

```sql
mysql> create database test1;	---创建数据库test1
mysql> show databases;			---查看所有数据库
mysql> use test1;				---选择数据库test1
mysql> show tables;				---查看数据库test1下所有表

mysql> drop database test1;		---删除数据库test1

mysql> 
create tabe emp(
ename 		varchar(10),
hiredate 	data,
sal 		decimal(10,2),
deptno 		int(10)
);						---创建表emp

mysql> desc emp;		---查看表emp定义
mysql> show create table emp \G;	---查看全面的表emp定义,\G表示格式按字段竖向排列

mysql> drop table emp;		---删除表emp

mysql> alter table emp modify ename varchar(20);	---修改字段ename类型
mysql> alter table emp add column age int(3);		---增加字段age
mysql> alter table emp drop column age;				---删除字段age
mysql> alter table emp change age age1 int(4);		---字段age改名为age1
mysql> alter table emp add birth date after ename;	---增加字段birth,在ename后
mysql> alter table emp modify age int(5) first;		---修改字段age类型，放最前面

mysql> alter table emp rename emp1;		---更高表名
```



### 3 DML

```sql
---插入insert
insert into emp(ename,hiredate,sal,deptno) value('lin','1985-04-19',20000,1);

---更新update
update emp set sal=50000 where ename='lin';

---删除delete
delete from emp where ename='chen';

---选择select
select * from emp where deptno=1;

---排序order by
---默认是升序，desc表示降序
select * from emp order by sal;
select * from emp order by sal desc;

---限制分页limit
select * from emp limit 3;
select * from emp limit 100,200;

---聚合sum, count, max, min, group by, having
---区别：where是对聚合前过滤，having是对聚合后过滤
select count(1) from emp;
select deptno,count(1) from emp group by deptno;
select deptno,count(1) from emp group by deptno having count(1)>10;
select sum(sal),max(sal),max(sal) from emp;

---内连接
---选择2表中相互匹配的内容
select ename,deptname from emp,dept where emp.deptno=dept.deptno;

---外连接
---左连接，包含左边表所有内容和右边表没有与其匹配的内容
---右连接，包含右边表所有内容和左边表没有与其匹配的内容
select ename,deptname from emp left join dept on emp.deptno=dept.deptno;
select ename,deptname from emp right join dept on emp.deptno=dept.deptno;

---子查询
---表连接性能优于子查询
select * from emp where deptno in(select deptno from dept);

---记录联合union, union all
---区别：union直接汇合，union all汇合后distinct
select deptno from emp
union all
select deptno from dept;
```



### 4  DCL

```sql
---grant
mysql> grant select on test1.* to 'lin'@'localhost' identified by '123456';

---revoke
mysql> revoke select on test1.* to 'lin'@'localhost';
```

