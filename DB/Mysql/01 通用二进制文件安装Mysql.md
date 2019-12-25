## 通用二进制文件安装Mysql



### 1 检查旧安装并删除

```shell
rpm -qa|grep mysql
rm -rf /etc/my.cnf
rm -rf /etc/mysql

rpm -qa|grep mariadb
rpm -e --nodeps mariadb-libs-5.5.44-2.el7.centos.x86_64
```







### 2 修改配置文件my.cnf

/etc/my.cnf

```
[mysqld]
port=3306
user=mysql
basedir=/app/mysql
datadir=/app/mysql/data
socket=/tmp/mysql.sock
log-error=/app/mysql/logs/mysql.err
pid-file=/app/mysql/data/mysql.pid
character_set_server=utf8mb4
explicit_defaults_for_timestamp=true
```



### 3 安装Mysql

```shell
groupadd mysql
useradd -r -g mysql mysql

cd /app
tar zxvf mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
ln -s mysql-5.7.26-linux-glibc2.12-x86_64 mysql

mkdir -p /app/mysql/data
mkdir -p /app/mysql/logs
chown mysql:mysql -R /app/mysql

# 初始化数据目录
cd /app/mysql
bin/mysqld --basedir=/app/mysql --datadir=/app/mysql/data --initialize --user=mysql
```



###  4 查看初始密码 

```
/app/mysql/logs/mysql.err
```



### 5 启动mysql

```shell
cp /app/mysql/support-files/mysql.server /etc/init.d/mysql
service mysql start
```



### 6 修改初始密码

```shell
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
```

