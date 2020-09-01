## Python与Mysql交互

### 1 pymysql模块

```python
# 导入pymysql模块
import pymysql
```



### 2 connect()对象

```python
# 建立与数据库的连接
conn = connect(host='localhost',port=3306,database='jing_dong',user='root',password='mysql',charset='utf8')
```

- host：连接的mysql主机，如果本机是'localhost'
- port：连接的mysql主机的端口，默认是3306
- database：数据库的名称
- user：连接的用户名
- password：连接的密码
- charset：通信采用的编码方式，推荐使用utf8

```python
# 关闭Connection对象
conn.close()
```



### 3 cursor()对象

execute()：执行select、insert、update、delete SQL

close()：关闭cursor对象

```python
# 获取cursor对象
cs1=conn.cursor()
```

```python
# 关闭cursor对象
cs1.close()
```



### 4 Insert/Update/Delete

```python
# insert SQL
cs1.execute('insert into goods_cates(name) values("硬盘")')
cs1.execute('insert into goods_cates(name) values("光盘")')
```

```python
# update SQL
cs1.execute('update goods_cates set name="机械硬盘" where name="硬盘"')
```

```python
# delete SQL
cs1.execute('delete from goods_cates where id=6')
```



```python
# insert,update,delete例子
import pymysql
 
def main():
    conn=pymysql.connect(host='sfpay-m.dbsit.sfcloud.local',port=3306,database='sfpay',user='sfpay',password='sfpay##2020',charset='utf8mb4')
    cs1=conn.cursor()
    cs1.execute('insert into goods_cates(name) values("硬盘")')
    cs1.execute('update goods_cates set name="机械硬盘" where name="硬盘"')
    cs1.execute('delete from goods_cates where id=3')
    conn.commit()
    cs1.close()
    conn.close()
 
if __name__=='__main__':
    main()
```



### 5 Select

fetchone()执行查询语句时，获取查询结果集的第一个行数据，返回一个元组。

fetchall()执行查询时，获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回。

```python
# select SQL，并返回受影响的行数
cs1.execute('select id,name from goods where id>=4')
cs1.fetchone()
cs1.fetchall()
```



```python
# fetchone()例子
import pymysql
 
def main():
    conn=pymysql.connect(host='sfpay-m.dbsit.sfcloud.local',port=3306,database='sfpay',user='sfpay',password='sfpay##2020',charset='utf8mb4')
    cs1=conn.cursor()
    cs1.execute('select * from goods where id<4')
    print(cs1.fetchone())
    conn.commit()
    cs1.close()
    conn.close()
 
if __name__=='__main__':
    main()
```

```python
# fetchall()例子
import pymysql
 
def main():
    conn=pymysql.connect(host='sfpay-m.dbsit.sfcloud.local',port=3306,database='sfpay',user='sfpay',password='sfpay##2020',charset='utf8mb4')
    cs1=conn.cursor()
    cs1.execute('select * from goods where id<4')
    print(cs1.fetchall())
    conn.commit()
    cs1.close()
    conn.close()
 
if __name__=='__main__':
    main()
```



### 6 传参

```python
# SQL传参
import pymysql
 
def main():
    name=input("请输入物品名称:")
    conn = pymysql.connect(host='sfpay-m.dbsit.sfcloud.local', port=3306, database='sfpay', user='sfpay',
                           password='sfpay##2020', charset='utf8mb4')
    cs1=conn.cursor()
    params=[name]
    cs1.execute('select * from goods_cates  where name=%s',params)
    print(cs1.fetchall())
    cs1.close()
    conn.close()
 
if __name__=='__main__':
    main()
```

