## Zabbix 5.0 LTS安装

Centos：7.8

Zabbix：5.0 LTS

服务端IP：192.168.100.11

客户端IP：192.168.100.12

![image-20201111101631759](D:\Notes\Other\Zabbix\image\image-20201111101631759.png)



Zabbix文档：https://www.zabbix.com/documentation/5.0/start



### 一、服务端安装

鉴于国内网络特殊情况，使用阿里云 zabbix 源安装。

#### 1 配置阿里源

```bash
rpm -Uvh https://mirrors.aliyun.com/zabbix/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
sed -i 's#http://repo.zabbix.com#https://mirrors.aliyun.com/zabbix#' /etc/yum.repos.d/zabbix.repo
yum clean all
```

#### 2  mariadb安装

```bash
# 安装 mariadb
yum install mariadb-server -y
 
# 启动 mariadb
systemctl start mariadb
 
# 创建 zabbix 数据库和用户
mysql -uroot -p
MariaDB [(none)]> create database zabbix character set utf8 collate utf8_bin;
MariaDB [(none)]> create user zabbix@localhost identified by '123456';
MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost;
```

#### 3 zabbix安装

```bash
# 安装 zabbix server 和 agent
yum install zabbix-server-mysql zabbix-agent -y
 
# 安装 Software Collections
yum install centos-release-scl -y
 
# 启用 zabbix 前端源
vi /etc/yum.repos.d/zabbix.repo
------------修改enabled值------------
[zabbix-frontend]
...
enabled=1
...
-------------------------------------
 
# 安装 zabbix 前端和相关环境
yum install zabbix-web-mysql-scl zabbix-apache-conf-scl -y
 
# 导入 zabbix 数据库
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
 
# 修改 zabbix 连数据库密码
vi /etc/zabbix/zabbix_server.conf
 
# 修改时区
vi /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf
----------------------------
php_value[date.timezone] = Asia/Shanghai
----------------------------
```

#### 4 启动验证

```bash
# 启动 zabbix-server、zabbix-agent、httpd、rh-php72-php-fpm
systemctl restart zabbix-server zabbix-agent httpd rh-php72-php-fpm
```

![img](D:\Notes\Other\Zabbix\image\6809900083c16a19f10b82673f5cc250.png)

![img](D:\Notes\Other\Zabbix\image\33083ef6c49f4a5f9acd2ec493b714b0.png)

![img](D:\Notes\Other\Zabbix\image\f659871443b0586bac6c22fc35c5ccf0.png)

![img](D:\Notes\Other\Zabbix\image\c8fde1e35ca00a60826bd9e73f0fc42e.png)

![img](D:\Notes\Other\Zabbix\image\dc23a18de1dbe46efe5a8cbd02a8f994.png)

![img](D:\Notes\Other\Zabbix\image\a51fb2e222198c501c4c3be5313d5350.png)

![img](D:\Notes\Other\Zabbix\image\b4440f223c320cca73110d99c037a283.png)



登录账号为 Admin，密码：zabbix



### 二、客户端安装

需配置阿里云源，参考上面 Zabbix 安装。

#### 1 Agent2安装

```bash
# Agent2 监控客户端安装
yum install zabbix-agent2 -y
 
# 修改配置文件
vi /etc/zabbix/zabbix_agent2.conf
-------------修改内容----------------------
Server=192.168.100.11
ServerActive=192.168.100.11
Hostname=node1
-------------------------------------------
 
# 启动 agent2
systemctl restart zabbix-agent2
```



