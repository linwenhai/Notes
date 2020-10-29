## Mysql环境搭建

二进制文件安装Mysql



### 1 下载二进制安装包

版本：mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz

清华大学镜像下载：https://mirror.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz

官网下载：https://dev.mysql.com/downloads/mysql/

1】点击 "Looking for previous GA versions?"

![img](D:\Notes\DB\Mysql\image\12475671-4644dbb77424c04a.png)

2】选择 "Linux - Generic" ， x86-64 bit 的版本

![image-20201021215939754](D:\Notes\DB\Mysql\image\image-20201021215939754.png)

![image-20201021220144684](D:\Notes\DB\Mysql\image\image-20201021220144684.png)



### 2 安装

```shell
# 删除旧环境
rpm -qa | grep mysql
rpm -qa | grep mariadb
rpm -e --nodeps <rpm包名>
rm -rf /etc/my.cnf
rm -rf /etc/mysql

# 安装依赖包
yum install libaio
yum install libtinfo.so.5

# 安装mysql
cd /opt
wget https://mirror.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz
tar -zxvf mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz
mv mysql-5.7.26-linux-glibc2.12-x86_64 mysql
```



### 3 配置

```shell
# 创建用户
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
mkdir -p /opt/mysql/data
mkdir -p /opt/mysql/logs
chown -R mysql:mysql /opt/mysql
```

```shell
# 修改配置文件
vi /etc/my.cnf
```

>[mysqld] 
>port=3306
>basedir=/opt/mysql
>datadir=/opt/mysql/data 
>socket=/opt/mysql/data/mysql.sock 
>pid-file = /opt/mysql/data/mysql.pid 
>log-error = /opt/mysql/logs/error.log 
>
>[mysql] 
>default-character-set=utf8mb4
>
>[client] 
>socket=/opt/mysql/logs/mysql.sock 
>default-character-set=utf8mb4

```shell
# 初始化数据目录，临时root密码在/opt/mysql/logs/error.log
bin/mysqld --initialize --user=mysql --basedir=/opt/mysql --datadir=/opt/mysql/data
```



### 4 启动/关闭

```bash
/opt/mysql/support-files/mysql.server start
/opt/mysql/support-files/mysql.server stop
/opt/mysql/support-files/mysql.server status
```



### 5 重置root密码

```bash
# 登陆mysql
bin/mysql -u root -p

# 重置root密码
mysql> set password=password('123456');
mysql> flush privileges;

# 授予root账户远程连接mysql
mysql> use mysql
mysql> select Host from user;
mysql> grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
mysql> flush privileges;
```



```bash
# 查看mysql版本
mysql> select version();
```



### 6 配置详解

| 参数                                  | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| port = 3306                           | Mysql端口                                                    |
| basedir = /opt/mysql                  | 指定Mysql安装的绝对路径                                      |
| datadir = /opt/mysql/data             | 指定Mysql数据存放的绝对路径                                  |
| socket = /tmp/mysql.sock              | 套接字文件                                                   |
| plugin_dir = /opt/mysql/lib/plugin    | plugin插件所在的路径                                         |
| log-error = /opt/mysql/logs/error.log | 错误日志存放的路径                                           |
| symbolic-links = 0                    | 符号连接，默认都是0                                          |
| local-infile = 0                      | 设置为0表示关闭服务器从本地load的功能，设置为1则打开         |
| max-connections = 1000                | 设置Mysql的最大连接数                                        |
| query_cache_limit = 4M                | 指定单个查询可以使用的缓冲区的大小，默认值是1M               |
| query_cache_size = 64M                | 查询的缓存大小设置                                           |
| query_cache_type = 1                  | 设置缓存的类型，0表示禁用缓存，1表示缓存所有结果             |
| max_user_connections = 1000           | 用户连接数的最大值设置                                       |
| wait_timeout = 9000                   | 超时等待时间，单位秒                                         |
| connect_timeout = 20                  | 客户端与服务器建立连接时，单位秒                             |
| thread_cache_size = 256               | 用于缓存空闲的线程                                           |
| key_buffer_size = 16M                 | 用于指定索引缓冲区的大小                                     |
| join_buffer_size = 2M                 | join查询缓存，默认2M                                         |
| max_heap_table_size = 16M             | 指定用户可创建内存表的大小；                                 |
| low_priority_updates = 1              | 设置服务器降低写操作的优先级，设置为1表示以读为主            |
| max_allowd_packet = 128M              | 设置一次消息传输的最大值                                     |
| max_seeks_for_key = 100               | 设置基于key查询允许的最大查找次数                            |
| sort_buffer_size = 16M                | 通过增加该值的大小可以提高查询中使用“group by”和“order by”的性能 |
| read_buffer_size = 16M                | 设置服务器读缓冲区的大小                                     |
| max_connect_errors = 10               | 客户端连接服务器在没有成功时就被阻断了                       |
| myisam_sort_buffer_size = 64M         | 服务器重建索引时允许建立的最大临时文件的大小                 |
| tmp_table_size = 64M                  | 设置临时内部堆积表（Heap）的大小                             |
| read_rnd_buffer_size = 1M             | 设置服务器随机读取缓冲区的大小                               |
| open_file_limit = 6050                | 控制文件打开的个数                                           |

