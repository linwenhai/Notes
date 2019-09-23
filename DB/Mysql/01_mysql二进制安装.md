## mysql二进制安装

### 1 下载编译好的二进制安装包

mysql-5.7.12-linux-glibc2.5-x86_64.tar

```shell
# yum安装所需相关依赖包
yum -y install gcc-c++ 
yum -y install zlib zlib-devel pcre pcre-devel
yum -y install openssl-devel
yum -y install libaio-devel.x86_64

# 自带mysql的mariadb
rpm -qa|grep mariadb
rpm -e mariadb-libs  --nodeps
```



### 2 解压包

```shell
tar -zxvf /app/install/mysql-5.7.12-linux-glibc2.5-x86_64.tar
ln -s mysql-5.7.18-linux-glibc2.5-x86_64 mysql
```



### 3 创建用户、目录、配置文件

```shell
groupadd mysql
useradd -r -g mysql mysql
mkdir -p /app/mysql/data
mkdir -p /app/mysql/log
chown -R mysql:mysql /app/mysql/data
vi /etc/my.cnf
```

>[client]
>port = 3306
>socket = /app/mysql/data/mysql.sock
>
>[mysqld]
>server_id=10
>port = 3306
>user = mysql
>character-set-server = utf8mb4
>default_storage_engine = innodb
>log_timestamps = SYSTEM
>socket = /app/mysql/data/mysql.sock
>basedir =/app/mysql
>datadir = /app/mysql/data
>pid-file = /app/mysql/data/mysql.pid
>max_connections = 1000
>max_connect_errors = 1000
>table_open_cache = 1024
>max_allowed_packet = 128M
>open_files_limit = 65535
>server-id=1
>gtid_mode=on
>enforce_gtid_consistency=on
>log-slave-updates=1
>log-bin=master-bin
>log-bin-index = master-bin.index
>relay-log = relay-log
>relay-log-index = relay-log.index
>binlog_format=row
>log_error = /app/mysql/log/mysql-error.log 
>skip-name-resolve
>log-slave-updates=1
>relay_log_purge = 0 
>slow_query_log = 1
>long_query_time = 1 
>slow_query_log_file = /app/mysql/log/mysql-slow.log



### 4 初始化数据库

```shell
/app/mysql/bin/mysqld --defaults-file=/etc/my.cnf --initialize --user=mysql --basedir=/app/mysql --datadir=/app/mysql/data --innodb_undo_tablespaces=3 --explicit_defaults_for_timestamp
```



### 5 查看初始密码

```shell
grep 'password' /app/mysql/log/mysql-error.log
```



### 6 创建ssl加密

```shell
/app/mysql/bin/mysql_ssl_rsa_setup --datadir=/app/mysql/data
```





### 7 启动/关闭mysql

```shell
/app/mysql/support-files/mysql.server stop
/app/mysql/support-files/mysql.server start
ps -ef|grep mysql
netstat -an|grep 3306
```



### 8 修改mysql的root用户密码

```shell
vi /etc/my.cnf
```

在[mysqld]这个条目下加入
skip-grant-tables

```shell
#启停mysql
/app/mysql/support-files/mysql.server stop
/app/mysql/support-files/mysql.server start

# 连接mysql服务器，-u后面跟用户名，-p表示需要输入密码
/app/mysql/bin/mysql -uroot -p


mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
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



### 9 创建测试hive用户

```shell
mysql> create user 'hive' identified by 'hive';
mysql> grant all on *.* TO 'hive'@'%' identified by 'hive' with grant option;
mysql> grant all on *.* TO 'hive'@'localhost' identified by 'hive' with grant option;
mysql> flush privileges;

mysql -uhive -phive		#登录测试
mysql> create database hive;
mysql> show databases;
```















