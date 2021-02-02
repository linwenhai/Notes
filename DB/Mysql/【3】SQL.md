## Mysql SQL



- **DDL**(Data Definition Languages)：数据定义语句，常用关键字create, drop, alter。
- **DML**(Data Manipulation Language)：数据操纵语句，常用关键字insert, delete, update, select。
- **DCL**(Data Control Language)：数据控制语句，常用关键字grant, revoke。

 

#### 一、DDL

```sql
show databases;    -- 查看数据库

create database test;    -- 创建库

drop database test;      -- 删除库

use test;    -- 进入test库

create table emp(
    ename varchar(10),
    hiredate date,
    sal decimal(10,2),
    deptno int(10));    -- 创建表emp

drop table emp;    -- 删除表

create table tmp01 like emp;    --备份表

insert into tmp01 select * from emp;

desc emp;    -- 查看表定义

show create table emp \G;

alter table emp modify ename varchar(20);    -- 修改字段

alter table emp add column age int(3);    -- 删除字段

alter table emp change age age1 int(4);    -- 修改字段名

alter table emp add birth date after ename;    -- 增加字段，指定位置

alter table emp modify age1 int(5) first;

alter table emp rename emp1;    -- 修改表名
```

 



#### 二、DML

```sql
insert into emp(ename,hiredate,sal,deptno) value('lin','1985-04-19',20000,1);
insert into emp(ename,hiredate,sal,deptno) value('xin','1988-06-29',10000,2);
insert into emp(ename,hiredate,sal,deptno) value('zhao','1989-01-23',12000,3);

update emp set sal=50000 where ename='lin';

delete from emp where ename='zhao';

select * from emp where deptno=1;

select * from emp order by sal desc;

select * from emp order by sal limit 10;

select count(1) from emp;

select deptno,count(1) from emp group by deptno;

select deptno,count(1) from emp group by deptno having count(1)>10;

select sum(sal),max(sal),min(sal) from emp;

select ename,deptname from emp,dept where emp.deptno=dept.deptno;    -- 内连接

select ename,deptname from emp left join dept on emp.deptno=dept.deptno;    -- 外连接

select ename,deptname from emp right join dept on emp.deptno=dept.deptno;

select * from emp where deptno in(select deptno from dept);    -- 子查询,表连接性能优于子查询

select deptno from emp
union all
select deptno from dept;
```

 



