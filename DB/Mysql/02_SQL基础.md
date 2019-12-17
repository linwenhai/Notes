## SQL基础

### 1 SQL分类

DDL(Data Definition Languages)：数据定义语句，常用关键字create, drop, alter。

DML(Data Manipulation Language)：数据操纵语句，常用关键字insert, delete, update, select。

DCL(Data Control Language)：数据控制语句，常用关键字grant, revoke。



### 2 DDL语句

```sql
---创建数据库test1
mysql> create database test1;
Query OK, 1 row affected (0.01 sec)

---删除数据库test1
mysql> drop database test1;
Query OK, 0 rows affected (0.01 sec)

---查看所有数据库
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test1              |
+--------------------+
5 rows in set (0.00 sec)

---选择数据库test1
mysql> use test1;
Database changed

---创建表emp
mysql> create table emp(ename varchar(10),hiredate date,sal decimal(10,2),deptno int(10));
Query OK, 0 rows affected (0.01 sec)

---删除表emp
mysql> drop table emp;
Query OK, 0 rows affected (0.01 sec)

---查看表emp定义
mysql> desc emp;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| ename    | varchar(10)   | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(10)       | YES  |     | NULL    |       |
+----------+---------------+------+-----+---------+-------+
4 rows in set (0.07 sec)

---查看全面的表emp定义,\G表示格式按字段竖向排列
mysql> show create table emp \G;
*************************** 1. row ***************************
       Table: emp
Create Table: CREATE TABLE `emp` (
  `ename` varchar(10) DEFAULT NULL,
  `hiredate` date DEFAULT NULL,
  `sal` decimal(10,2) DEFAULT NULL,
  `deptno` int(10) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
1 row in set (0.00 sec)

---修改字段ename类型
mysql> alter table emp modify ename varchar(20);
Query OK, 0 rows affected (0.01 sec)

---增加字段age
mysql> alter table emp add column age int(3);
Query OK, 0 rows affected (0.02 sec)

---删除字段age
mysql> alter table emp drop column age;
Query OK, 0 rows affected (0.02 sec)

---字段age改名为age1
mysql> alter table emp change age age1 int(4);
Query OK, 0 rows affected (0.01 sec)

---增加字段birth,在ename后
mysql> alter table emp add birth date after ename;
Query OK, 0 rows affected (0.03 sec)

---修改字段age1类型，放最前面
mysql> alter table emp modify age1 int(5) first;
Query OK, 0 rows affected (0.03 sec)

---更改表名
mysql> alter table emp rename emp1;
Query OK, 0 rows affected (0.02 sec)
```





### 3 DML语句

```sql
---插入insert
mysql> insert into emp(ename,hiredate,sal,deptno) value('lin','1985-04-19',20000,1);
Query OK, 1 row affected (0.01 sec)
mysql> insert into emp(ename,hiredate,sal,deptno) value('zhang','1988-05-11',10000,2);
Query OK, 1 row affected (0.01 sec)
mysql> insert into emp(ename,hiredate,sal,deptno) value('li','1989-01-23',12000,3);
Query OK, 1 row affected (0.01 sec)
mysql> insert into emp(ename,hiredate,sal,deptno) value('zhao','1990-02-17',18000,4);
Query OK, 1 row affected (0.01 sec)

---更新update
mysql> update emp set sal=50000 where ename='lin';
Query OK, 1 row affected (0.01 sec)

---删除delete
mysql> delete from emp where ename='lin';
Query OK, 1 row affected (0.01 sec)

---选择select
mysql> select * from emp where deptno=1;
+------+-------+-------+------------+----------+--------+
| age1 | ename | birth | hiredate   | sal      | deptno |
+------+-------+-------+------------+----------+--------+
| NULL | lin   | NULL  | 1985-04-19 | 20000.00 |      1 |
+------+-------+-------+------------+----------+--------+
1 row in set (0.00 sec

---排序order by
---默认是升序，desc表示降序
mysql> select * from emp order by sal;
+------+-------+-------+------------+----------+--------+
| age1 | ename | birth | hiredate   | sal      | deptno |
+------+-------+-------+------------+----------+--------+
| NULL | zhang | NULL  | 1988-05-11 | 10000.00 |      2 |
| NULL | lin   | NULL  | 1985-04-19 | 20000.00 |      1 |
+------+-------+-------+------------+----------+--------+
2 rows in set (0.00 sec)
              
mysql> select * from emp order by sal desc;
+------+-------+-------+------------+----------+--------+
| age1 | ename | birth | hiredate   | sal      | deptno |
+------+-------+-------+------------+----------+--------+
| NULL | lin   | NULL  | 1985-04-19 | 20000.00 |      1 |
| NULL | zhang | NULL  | 1988-05-11 | 10000.00 |      2 |
+------+-------+-------+------------+----------+--------+
2 rows in set (0.00 sec)

---限制分页limit
mysql>  select * from emp order by sal limit 1;
+------+-------+-------+------------+----------+--------+
| age1 | ename | birth | hiredate   | sal      | deptno |
+------+-------+-------+------------+----------+--------+
| NULL | zhang | NULL  | 1988-05-11 | 10000.00 |      2 |
+------+-------+-------+------------+----------+--------+
1 row in set (0.00 sec)
---限制分页limit,从第2条记录开始的3条记录
mysql> select * from emp order by sal limit 1,3;
+------+-------+-------+------------+----------+--------+
| age1 | ename | birth | hiredate   | sal      | deptno |
+------+-------+-------+------------+----------+--------+
| NULL | li    | NULL  | 1989-01-23 | 12000.00 |      3 |
| NULL | zhao  | NULL  | 1990-02-17 | 18000.00 |      4 |
| NULL | lin   | NULL  | 1985-04-19 | 20000.00 |      1 |
+------+-------+-------+------------+----------+--------+
3 rows in set (0.00 sec)


---聚合sum, count, max, min, group by, having
---区别：where是对聚合前过滤，having是对聚合后过滤
mysql> select count(1) from emp;
+----------+
| count(1) |
+----------+
|        4 |
+----------+
1 row in set (0.00 sec)
              
mysql> select deptno,count(1) from emp group by deptno;
+--------+----------+
| deptno | count(1) |
+--------+----------+
|      1 |        1 |
|      2 |        1 |
|      3 |        1 |
|      4 |        1 |
+--------+----------+
4 rows in set (0.00 sec)
              
mysql> select deptno,count(1) from emp group by deptno having count(1)>10;
              
mysql> select sum(sal),max(sal),min(sal) from emp;
+----------+----------+----------+
| sum(sal) | max(sal) | min(sal) |
+----------+----------+----------+
| 60000.00 | 20000.00 | 10000.00 |
+----------+----------+----------+
1 row in set (0.00 sec)

---内连接
---选择2表中相互匹配的内容
mysql> select ename,deptname from emp,dept where emp.deptno=dept.deptno;

---外连接
---左连接，包含左边表所有内容和右边表没有与其匹配的内容
---右连接，包含右边表所有内容和左边表没有与其匹配的内容
mysql> select ename,deptname from emp left join dept on emp.deptno=dept.deptno;
mysql> select ename,deptname from emp right join dept on emp.deptno=dept.deptno;

---子查询
---表连接性能优于子查询
mysql> select * from emp where deptno in(select deptno from dept);

---记录联合union, union all
---区别：union直接汇合，union all汇合后distinct
mysql> 	select deptno from emp
		union all
		select deptno from dept;
```



### 4  DCL语句

```sql
---grant授权
mysql> grant select on test1.* to 'lin'@'localhost' identified by '123456';
Query OK, 0 rows affected, 2 warnings (0.01 sec)

---revoke回收权限
mysql> revoke select on test1.* from 'lin'@'localhost';
Query OK, 0 rows affected, 1 warning (0.01 sec)
```

