## 使用通用二进制文件在Unix / Linux上安装MySQL

### 1 删除旧环境

```shell
rpm -qa | grep mysql
rpm -qa | grep mariadb
rm -rf /etc/my.cnf
rm -rf /etc/mysql
```



### 2 安装依赖包

```shell
yum install libaio
yum install libtinfo.so.5
```



### 3 安装mysql

```shell
cd /opt
tar zxvf mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
ln -s mysql-5.7.26-linux-glibc2.12-x86_64 mysql
```



### 4 创建用户

```shell
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
mkdir -p /opt/mysql/data
chown -R mysql:mysql /opt/mysql
```



### 5 创建配置文件

```shell
vi /etc/my.cnf
```

```
[mysqld]
port=3306
basedir=/opt/mysql
datadir=/opt/mysql/data
socket=/opt/mysql/data/mysql.sock
pid-file = /data/mysql/data/mysql.pid
log-error = /data/mysql/logs/error.log
key_buffer_size=16M
max_allowed_packet=8M

[mysql]
default-character-set=utf8mb4

[client]
socket=/opt/mysql/data/mysql.sock
default-character-set=utf8mb4
```





### 6 初始化数据目录

```shell
bin/mysqld --initialize --user=mysql --basedir=/opt/mysql --datadir=/opt/mysql/data
```



### 7 添加 mysql 服务

```shell
cp support-files/mysql.server /etc/init.d/mysql
chkconfig --add mysql
chkconfig --list
```



### 8 配置环境变量

```shell
vi /etc/profile
```

添加内容：

```
export MYSQL_HOME=/opt/mysql
export PATH=$MYSQL_HOME/bin:$PATH
```

```shell
source /etc/profile
```



### 9 启动/停止mysql

```shell
service mysql start
service mysql stop
service mysql status
ps -ef|grep mysql
```



### 10 重置root密码

```shell
mysql -u root -p
```

```sql
mysql> set password=password('mysql');
mysql> grant all privileges on *.* to root@'%' identified by 'mysql';
mysql> flush privileges;
```



### 11 查看版本

```shell
mysql> select version();
```

