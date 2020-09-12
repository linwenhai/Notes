## Mysql SQL

### 1 SQL分类

- **DDL**(Data Definition Languages)：数据定义语句，常用关键字create, drop, alter。
- **DML**(Data Manipulation Language)：数据操纵语句，常用关键字insert, delete, update, select。
- **DCL**(Data Control Language)：数据控制语句，常用关键字grant, revoke。

### 2 DDL语句

```sql
/*1.查看所有数据库*/
show databases;
```

```sql
/*2.创建、删除数据库*/
create database test;
drop database test;
```

```sql
/*3.进入数据库*/
use test;
```

```sql
/*4.创建、删除表*/
create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(10));
drop table emp;
```

```sql
/*5.备份表*/
create table tbl_bill_20200803 like tbl_bill;
insert into tbl_bill_20200803 select * from tbl_bill;

create table tmp01 like emp;
insert into tmp01 select * from emp;
```

```sql
/*6.查看表定义*/
desc emp;
show create table emp \G;
```

```sql
/*7.修改字段类型*/
alter table emp modify ename varchar(20);
```

```sql
/*8.增加字段*/
alter table emp add column age int(3);
```

```sql
/*9.删除字段*/
alter table emp drop column age;
```

```sql
/*10.修改字段名*/
alter table emp change age age1 int(4);
```

```sql
/*11.增加字段，指定位置*/
alter table emp add birth date after ename;
alter table emp modify age1 int(5) first;
```

```sql
/*12.修改表名*/
alter table emp rename emp1;
```

### 3 DML语句

```sql
/*1.insert*/
insert into emp(ename,hiredate,sal,deptno) value('lin','1985-04-19',20000,1);
insert into emp(ename,hiredate,sal,deptno) value('xin','1988-06-29',10000,2);
insert into emp(ename,hiredate,sal,deptno) value('zhao','1989-01-23',12000,3);
```

```sql
/*2.update*/
update emp set sal=50000 where ename='lin';
```

```sql
/*3.delete*/
delete from emp where ename='zhao';
```

```sql
/*4.select*/
select * from emp where deptno=1;
```

```sql
/*5.order by,默认是升序,desc表示降序*/
select * from emp order by sal;
select * from emp order by sal desc;
```

```sql
/*6.limit,限制分页*/
select * from emp order by sal limit 1;
select * from emp order by sal limit 1,3;
```

```sql
/*7.
聚合sum, count, max, min, group by, having
区别：where是对聚合前过滤，having是对聚合后过滤
*/
select count(1) from emp;
select deptno,count(1) from emp group by deptno;
select deptno,count(1) from emp group by deptno having count(1)>10;
select sum(sal),max(sal),min(sal) from emp;
```

```sql
/*8.内连接*/
select ename,deptname from emp,dept where emp.deptno=dept.deptno;
```

```sql
/*9.外连接
左连接，包含左边表所有内容和右边表没有与其匹配的内容
右连接，包含右边表所有内容和左边表没有与其匹配的内容
*/
select ename,deptname from emp left join dept on emp.deptno=dept.deptno;
select ename,deptname from emp right join dept on emp.deptno=dept.deptno;
```

```sql
/*10.子查询,表连接性能优于子查询*/
select * from emp where deptno in(select deptno from dept);
```

```sql
/*11.union, union all
区别：union直接汇合，union all汇合后distinct
*/
select deptno from emp
union all
select deptno from dept;
```

### 4 DCL语句