## Mysql SQL



- **DDL**(Data Definition Languages)：数据定义语句，常用关键字create, drop, alter。
- **DML**(Data Manipulation Language)：数据操纵语句，常用关键字insert, delete, update, select。
- **DCL**(Data Control Language)：数据控制语句，常用关键字grant, revoke。

 

#### 一、DDL

```sql
mysql> show databases;    -- 查看数据库
mysql> create database test;    -- 创建库
mysql> drop database test;      -- 删除库

mysql> use test;    -- 进入test库
create table emp(
    ename varchar(10),
    hiredate date,
    sal decimal(10,2),
    deptno int(10));    -- 创建表emp

mysql> drop table emp;    -- 删除表

mysql> create table tmp01 like emp;    --备份表
mysql> insert into tmp01 select * from emp;

mysql> desc emp;    -- 查看表定义
mysql> show create table emp \G;

mysql> alter table emp modify ename varchar(20);    -- 修改字段
mysql> alter table emp add column age int(3);    -- 增加字段
mysql> alter table emp change age age1 int(4);    -- 修改字段名
mysql> alter table emp add birth date after ename;    -- 增加字段，指定位置
mysql> alter table emp modify age1 int(5) first;
mysql> alter table emp rename emp1;    -- 修改表名
```

 



#### 二、DML

```sql
mysql> insert into emp(ename,hiredate,sal,deptno) value('lin','1985-04-19',20000,1);
mysql> insert into emp(ename,hiredate,sal,deptno) value('xin','1988-06-29',10000,2);
mysql> insert into emp(ename,hiredate,sal,deptno) value('zhao','1989-01-23',12000,3);

mysql> update emp set sal=50000 where ename='lin';		--update

mysql> delete from emp where ename='zhao';		--delete

mysql> select * from emp where deptno=1;	--select
mysql> select * from emp order by sal desc;
mysql> select * from emp order by sal limit 10;
mysql> select count(1) from emp;

mysql> select deptno,count(1) from emp group by deptno;		--group by
mysql> select deptno,count(1) from emp group by deptno having count(1)>10;

mysql> select sum(sal),max(sal),min(sal) from emp;

mysql> select ename,deptname from emp,dept where emp.deptno=dept.deptno;    -- 内连接
mysql> select ename,deptname from emp left join dept on emp.deptno=dept.deptno;    -- 外连接
mysql> select ename,deptname from emp right join dept on emp.deptno=dept.deptno;

mysql> select * from emp where deptno in(select deptno from dept);    -- 子查询,表连接性能优于子查询

mysql> select deptno from emp
union all
select deptno from dept;
```

 



