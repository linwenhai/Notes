## Linux常用命令

### 1 vi

1】正常模式

以 vi打开一个档案就直接进入一般模式了(这是默认的模式)

```
G		# 移动到文件的最后一行
gg      # 光标移动到文件的第一行
```

2】命令行模式

```
:q!				# 不存盘退出
:wq				# 存盘退出
:set number		# 显示行号
```

3】编辑模式

按下i, I, o, O, a, A, r, R等任何一个字母之后才会进入编辑模式



### 2 env

```shell
# 显示当前环境变量
env
```



### 3 echo

```shell
# 显示PATH变量值
echo $PATH
```



### 4 visduo

```shell
# 配置sudo权限
visudo
```

```
--------------添加如下内容-----------------
appdeploy   ALL=(ALL) NOPASSWD: /app/ufida/start.sh
appdeploy   ALL=(ALL) NOPASSWD: /app/ufida/stop.sh
```



### 5 curl

```
语法：curl [option] [url]
```

常见参数：

```
-I 仅测试HTTP头
-m 10 最多查询10s
-o /dev/null 屏蔽原有输出信息
-s silent模式，不输出任何东西
-w %{http_code} 控制额外输出
```

```shell
# 测试网页，查看HTTP响应头信息
curl -I http://www.baidu.com
# 只返回响应码
curl -I -m 10 -o /dev/null -s -w %{http_code} http://www.baidu.com
# 测试网页返回值
curl -o /dev/null -s -w %{http_code} www.linux.com

# 下载文件
curl -O http://www.linux.com/hello.sh

# 通过ftp下载文件
curl -O -u 用户名:密码 ftp://www.linux.com/1.txt
# 通过ftp上传文件
curl -T 1.txt -u 用户名:密码 ftp://www.linux.com/
```



### 6 wget

```
参数：
-O			# 下载文件重命名
-b			# 后台下载，不显示详细信息
--spider	# 测试URL
-o			# 输出信息保存到文件
-q			# 不显示输出信息
```

```shell
# 在当前目录下载文件
wget ftp://elk:Elk123456@10.110.39.241/filebeat-7.2.1.zip

# 测试是否能正常访问
wget --spider http://www.baidu.com
```



### 7 find

```shell
# 查找以txt结尾的文件名
find /app/logs -name "*.txt"
# 忽略大小写
find /app/logs -iname "*.txt"

# 查找7天前文件
find /app/logs -type f -mtime +7

# 搜索大于100MB的文件
find /app/logs -type f -size +100M


# 删除7天前的log
find /app/logs -type f -mtime +7 -name "*.log" -exec rm -rf {} \;

# 查找目录是否含有“10.116.39.222”内容的文件
find /app/logs -type f  -exec grep -H  "10.116.39.222" {} \;    
```



### 8 xargs

xargs的默认命令是echo，空格是默认定界符， 默认替换符号是{} 。

```shell
# 删除目录下所有log
ls *.log |xargs rm -rf {}

# kill所以Java进程
ps -ef|grep java|awk '{print $2}'|xargs kill -9
```



### 9 awk

语法：awk '{pattern + action}' {filenames}
pattern：表示AWK在数据中查找的内容
action：	是在找到匹配内容时所执行的一系列命令

```shell
cat /etc/passwd |awk -F ':' '{print $1}'
```

```
$0：则表示所有域，$1表示第一个域，$n表示第n个域。
-F：指定域分隔符为':'，默认每行按空格或TAB分割。
```



### 10 sort

sort将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按ASCII码值进行比较，最后将他们按升序输出。

参数	描述

```
-b	忽略每行前面开始出的空格字符
-c	检查文件是否已经按照顺序排序
-d	排序时，处理英文字母、数字及空格字符外，忽略其他的字符
-f	排序时，将小写字母视为大写字母
-i	排序时，除了040至176之间的ASCII字符外，忽略其他的字符
-m	将几个排序号的文件进行合并
-M	将前面3个字母依照月份的缩写进行排序
-n	依照数值的大小排序
-o	将排序后的结果存入制定的文件
-r	以相反的顺序来排序
-t	指定排序时所用的栏位分隔字符
```



### 11 uniq

uniq 命令用于报告或忽略文件中的重复行，一般与sort命令结合使用。

```shell
# sort排序
# uniq -c去重
# 按重复次数排序
cat access.log|awk '{print $4}'|awk -F '/' '{print $3}'|uniq -c|sort -nr|head -n 10
```



### 12 sed

语法：sed [option] 'sed command' filename

```
-n ：只打印模式匹配的行
-e ：直接在命令行模式上进行sed动作编辑，此为默认选项
-f ：将sed的动作写在一个文件内，用–f filename 执行filename内的sed动作
-r ：支持扩展表达式
-i ：直接修改文件内容

^   匹配行开始，如：/^sed/匹配所有以sed开头的行。
$   匹配行结束，如：/sed$/匹配所有以sed结尾的行。
[]  匹配一个指定范围内的字符。
[^] 匹配一个不在指定范围内的字符。
\   转义字符
```

```shell
# 只显示文件第2行
sed -n '2p' error.log
# 只显示文件第1到第3行
sed -n '1,3p' error.log

# 显示匹配sencond字符的行
sed -n '/second/p' error.log
# 显示匹"2019/04/19 03:50:55"配行，使用转义符
sed -n '/2019\/04\/19 03:50:55/p' error.log

# 显示'/起始时间/,/结束时间/p' 的行
sed -n '/03:29:59/,/03:57:16/p' error.log

# 替换字符串，分隔符为"#"
sed -i 's#/etc/logs/access.log#/app/logs/access.log#' filebeat.yml
```



### 13 grep

```
语法：grep [选项][模式][文件]
```

```
选项：
-c	--只输出匹配行的数量
-i	--搜索时忽略大小写
-h	--查询多文件时不显示文件名
-l	--只列出符合匹配的文件名，而不列出具体的匹配行
-n	--列出所有的匹配行，并显示行号
-r	--递归搜索，包含当前目录和子目录
-v	--不包含模式的所有行
```

```shell
grep -n "certif" 1.pem	#显示匹配"certif"的所有行，并显示行号
grep -v "certif" 1.pem	#显示不匹配"certif"的所有行
grep -i "certif" 1.pem	#显示匹配"certif"的所有行，忽略大小写
grep -l "certif" *	#显示匹配"certif"的文件名，只列出符合匹配的文件名，而不列出具体的匹配行
grep -r "certif" *	#递归搜索，包含当前目录和子目录
```



### 14 ps

- PID: 运行着的命令(CMD)的进程编号
- TTY: 命令所运行的位置（终端）
- TIME: 运行着的该命令所占用的CPU处理时间
- CMD: 该进程所运行的命令

```
参数：
a ：显示现行终端机下的所有进程，包括其他用户的进程
u ：以用户为主的进程状态
x ：通常与a这个参数一起使用，可列出较完整信息
-e ：显示所有进程,环境变量
-f ：做一个更为完整的输出
--no-headers ：不打印开头
```

```shell
ps aux|less
ps aux --sort=-%cpu|head -n 20		#按cpu降序排列
ps aux --sort=-rss|head -n 20		#按内存降序排列
ps -C getty		#显示一个名为getty的进程的信息
ps -f -C getty
ps -L 1213		#根据PID查看线程
ps aux --no-headers|less
```

```shell
ps aux|grep "frmweb"|awk -F ':' '{print $1}'|awk '{if($10>3600) print $2}'|xargs kill -9
```



### 15 tar

错误写法：

```shell
tar -zcvf tomcat.tar.gz --exclude=tomcat/logs/ --exclude=tomcat/libs/ tomcat
```



正确写法：

```shell
tar -zcvf tomcat.tar.gz --exclude=tomcat/logs --exclude=tomcat/libs tomcat
```



### 16 rpm

```shell
# 安装example.rpm包并在安装过程中显示正在安装的文件信息及安装进度
rpm -ivh example.rpm

# 查看 tomcat4 是否被安装
rpm -qa | grep tomcat4 

# --nodeps 忽略依赖关系并继续操作
# --force 强制操作 如强制安装删除等
rpm -e mariadb-libs  --nodeps
```



