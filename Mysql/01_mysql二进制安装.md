## mysql二进制安装

#### 1 下载编译好的二进制安装包

mysql-5.7.12-linux-glibc2.5-x86_64.tar



#### 2 解压

```shell
tar -zxvf /app/install/mysql-5.7.12-linux-glibc2.5-x86_64.tar
mv mysql-5.7.18-linux-glibc2.5-x86_64 mysql
```



#### 3 创建用户、目录、配置文件

```shell
groupadd mysql
useradd -r -g mysql mysql
mkdir -p /app/mysql/data
chown -R mysql:mysql /app/mysql/data
vi /etc/my.cnf
```

```
[mysql]
no-auto-rehash
default-character-set=utf8

[mysqld]
basedir = /app/mysql
datadir = /app/mysql/data
port = 3306
socket = /app/mysql/data/mysql.sock
character-set-server=utf8

[client]
port = 3306
default-character-set=utf8
socket = /app/mysql/data/mysql.sock
```



#### 4 安装数据库

```shell
cd /app/mysql
chown -R mysql:mysql ./
cd /app/mysql/bin
./mysqld --initialize --user=mysql --basedir=/app/mysql --datadir=/app/mysql/data
chown -R mysql:mysql /app/mysql
```



#### 5 启动/关闭

```shell
/app/mysql/support-files/mysql.server stop
/app/mysql/support-files/mysql.server start
ps -ef|grep mysql
netstat -an|grep 3306
```



#### 6 修改mysql的root用户密码

```shell
vi /etc/my.cnf
```

在[mysqld]这个条目下加入
skip-grant-tables

```shell
/app/mysql/support-files/mysql.server stop
/app/mysql/support-files/mysql.server start
/app/mysql/bin/mysql -uroot -p
mysql> set password=password('sf123456');
mysql> flush privileges;
```

```shell
vi /etc/my.cnf
```

在[mysqld]这个条目下删除
skip-grant-tables

```shell
/app/mysql/support-files/mysql.server stop
/app/mysql/support-files/mysql.server start
```



#### 7 创建hive用户

```shell
mysql> create user 'hive' identified by 'hive';
mysql> grant all on *.* TO 'hive'@'%' identified by 'hive' with grant option;
mysql> grant all on *.* TO 'hive'@'localhost' identified by 'hive' with grant option;
mysql> flush privileges;

mysql -uhive -phive		#登录测试
mysql> create database hive;
mysql> show databases;
```















