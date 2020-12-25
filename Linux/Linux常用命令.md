## 常用命令

### 一、压缩命令

#### 1 zip

```shell
zip -r html.zip /app/html    # 打包目录/app/html，参数r，递归操作
unzip html.zip    # 解压
```



#### 2 tar

```shell
tar -cvf 1.tar 1.log	#打包
tar -xvf 1.tar			#解压
```

```shell
tar -zcvf 1.tar.gz 1.log	#压缩打包
tar -zxvf 1.tar.gz			#解压
```

```bash
tar -jcvf 1.tar.bz2 *.log    # 打包
tar -jxvf 1.tar.bz2       	 # 解压
```



#### 3 gzip

```shell
gzip -r 1.tar 1.tar.gz	# 压缩文件
gzip -d 1.tar.gz		# 解压文件
```



### 二、上传/下载命令

#### 1 rz/sz

```shell
yum install lrzsz    # 安装lrzsz
rz          # 上传文件
sz 1.log    # 下载文件
```



#### 2 curl

```shell
# 获取网站http状态码
curl -I -m 5 -o /dev/null -s -w '%{http_code}\n' http://www.baidu.com
--------------------------------
-I ：仅测试HTTP头
-m 5 ：最多查询5s
-o /dev/null ：屏蔽原有输出信息
-s ：不输出任何东西
-w %{http_code} ：控制额外输出
--------------------------------
 
# 上传文件到FTP服务器
curl -u test:123456 -T 1.txt ftp://192.168.100.11/
 
# 下载FTP服务器文件
curl -u test:123456 ftp://192.168.100.11/logback.xml
```



#### 3 wget

wget参数：

- -O : 下载并以不同的文件名保存
- --spider : 模拟下载，不会下载，只是会检查是否网站是否好着

```shell
# 下载文件，将文档写入FILE（重命名）
wget -O [newname] [URL]
```

```shell
wget --spider http://www.baidu.com
```



### 三、文件操作命令

#### 1 grep

| 参数选项 | 含义                             |
| :------- | :------------------------------- |
| -v       | 显示不包含匹配“info”文本的所有行 |
| -n       | 显示匹配行及行号                 |
| -i       | 忽略大小写                       |
| -r       | 递归搜索，包含当前目录和子目录   |
| ^m       | 搜寻以m开头的行                  |
| e$       | 搜寻以 e 结束的行                |

```bash
grep -V "info" 1.log	# 显示不包含匹配 “info” 文本的所有行
grep -n "ERROR" 1.log	# 显示匹配行及行号
grep -i "error" 1.log	# 忽略大小写
grep -r "warn" *	# 递归搜索，包含当前目录和子目录
grep "^m" 1.log		# 搜寻以 m 开头的行
grep "e$" 1.log		# 搜寻以 e 结束的行
```



#### 2 find

```bash
find /tmp -name '*.sh'    # 查找/tmp目录下所有后缀为.sh的文件
find /tmp -name "[A-Z]*"    # 查找/tmp目录下所有以大写字母开头的文件
find /tmp -size 2M    # 查找/tmp 目录下等于2M的文件
find /tmp -size +2M   # 查找/tmp 目录下大于2M的文件
find /tmp -size -2M   # 查找/tmp 目录下小于2M的文件
find /tmp -size +4k -size -5M    # 查找/tmp目录下大于4k，小于5M的文件
find /tmp -type f -mtime +7    # 查找/tmp目录下7天前文件
```



#### 3 awk

语法：awk '{pattern + action}' {filenames}

- pattern：表示awk在数据中查找的内容。
- action：  是在找到匹配内容时所执行的一系列命令。
- $0：则表示所有域，$1表示第一个域，$n表示第n个域。
- -F：指定域分隔符为':'，默认每行按空格或TAB分割。

```shell
# 以冒号作为分隔符，打印第2域内容
awk -F ':' '{print $2}' 1.log
```

```shell
# 使用if判断语句
awk '{if ($12 > 1000) print $0}' access.log|more
```



#### 4 sort

**sort**将文件的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按ASCII码值进行比较，最后将他们按升序输出。

- -b : 忽略每行前面开始出的空格字符
- -c : 检查文件是否已经按照顺序排序

- -d : 排序时，处理英文字母、数字及空格字符外，忽略其他的字符

- -f : 排序时，将小写字母视为大写字母

- -i : 排序时，除了040至176之间的ASCII字符外，忽略其他的字符

- -m : 将几个排序号的文件进行合并

- -M : 将前面3个字母依照月份的缩写进行排序

- -n : 依照数值的大小排序

- -o : 将排序后的结果存入制定的文件

- -r : 以相反的顺序来排序
- -t : 指定排序时所用的栏位分隔字符

```bash
grep "ERROR" 1.log|sort -nr    # 按数值倒序排列
```



#### 5 uniq

uniq 命令用于报告或忽略文件中的重复行，一般与sort命令结合使用。

```shell
grep "eims" 1.log|uniq -c    # 去重
```



#### 6 sed

```bash
sed -n '2p' error.log        # 显示文件第2行
sed -n '1,3p' error.log      # 显示文件第1到第3行
sed -n '/sfpay/p' error.log    # 显示匹配 sfpay 关键字的行
sed -n '/2019\/04\/19 03:50:55/p' error.log    # 使用转义符
sed -n '/03:29:59/,/03:57:16/p' error.log      # 显示'/起始时间/,/结束时间/p' 的行
 
# 替换字符串，分隔符为"#"
sed -i 's#/etc/logs/access.log#/app/logs/access.log#' filebeat.yml
 
# 文件abc.txt，每行行首添加字符串duoduo
sed 's/^/duoduo/' abc.txt  >> test.txt
 
# 文件abc.txt，每行行尾添加字符串duoduo
sed 's/$/duoduo/' abc.txt  >> test.txt
```



#### 7 split

```shell
split -l 100000 nc.csv -d nc_    # 切割csv文件，切割后文件名为nc_**
rename nc* nc*.csv    # 批量重命名
-----------------------------------------
- -l : 按输出文件行数。
- -b : 按输出文件大小
- -d : 后缀为数字，默认为字母
- -a : 默认为2，意思是后缀的位数
-----------------------------------------
```



#### 8 scp

ssh scp等消除每次问yes/no方法

```shell
vi /etc/ssh/ssh_config
```

```shell
#  StrictHostKeyChecking ask 改成 no
StrictHostKeyChecking no
```



#### 9 vi

| 移动光标 | 描述                     |
| :------- | :----------------------- |
| gg       | 移到文章的开头           |
| G        | 移动到文章的最后         |
| $        | 移动到光标所在行的"行尾" |
| ^        | 移动到光标所在行的"行首" |
| dd       | 删除光标所在行           |
| yy       | 复制光标所在行           |
| p        | 粘贴                     |



### 四、时间任务命令

#### 1 date

```shell
date "+%Y-%m-%d %H:%M:%S"    # 格式化时间
 
time=`date "+%Y-%m-%d %H:%M:%S"`    # shell变量设置
```



#### 2 crontab

```shell
crontab -l    # 查看
crontab -e    # 编辑
```

```shell
# 语法
*    *    *    *    *    /bin/ls
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 7) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
```

```shell
# 每分钟执行一次
* * * * * sh test.sh

# 每5分钟执行一次
*/5 * * * * sh test.sh
```



### 五、进程相关命令

#### 1 xargs

xargs的默认命令是echo，空格是默认定界符，默认替换符号是{}。

xargs命令每次只获取一部分文件而不是全部。

```shell
# kill掉java进程
ps -ef|grep java|grep -v grep|awk '{print $2}'|xargs kill -9
 
# 删除当前目录下的log文件，空格是默认定界符，默认替换符号是{}
ls *.log|xargs rm -rf {}
```



#### 2 tcpdump

```shell
# 监视所有送到主机hostname的数据包
tcpdump -i ens33 dst host 192.168.198.11 >/tmp/1.log
```

```shell
# 截获主机hostname发送的所有数据
tcpdump -i ens33 src host 192.168.198.11 >/tmp/1.log
```



#### 3 top

```shell
top		# 查看OS所有进程状态
top -Hp <PID>		# 查看单个进程各个线程状态
```

