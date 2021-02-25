## Linux





#### 1 vi命令

| 移动光标 | 描述                     |
| :------- | :----------------------- |
| gg       | 移到文章的开头           |
| G        | 移动到文章的最后         |
| $        | 移动到光标所在行的"行尾" |
| ^        | 移动到光标所在行的"行首" |
| dd       | 删除光标所在行           |
| yy       | 复制光标所在行           |
| p        | 粘贴                     |





#### 2 xargs命令

```bash
# kill掉java进程
ps -ef|grep java|grep -v grep|awk '{print $2}'|xargs kill -9
 
# 删除当前目录下的log文件，空格是默认定界符，默认替换符号是{}
ls *.log|xargs rm -rf {}
```





#### 3 ps命令

| 参数          | 描述                                         |
| ------------- | -------------------------------------------- |
| -C <指令名称> | 指定执行指令的名称，并列出该指令的程序的状况 |
| -e            | 显示所有程序                                 |
| -f            | 显示UID,PPIP,C与STIME栏位                    |
| --no-header   | 不显示标题列                                 |

```bash
ps -ef|grep node		# 显式进程
```

```bash
ps -C node
```

>PID 		 TTY          TIME 		CMD
>115437	pts/0       00:00:52  node





#### 4 grep命令

```bash
# 显示不包含匹配 “info” 文本的所有行
grep -V "info" 1.log
 
# 显示匹配行及行号
grep -n "ERROR" 1.log
 
# 忽略大小写
grep -i "error" 1.log
 
# 递归搜索，包含当前目录和子目录
grep -r "warn" *
 
# 搜寻以 m 开头的行
grep "^m" 1.log
 
# 搜寻以 e 结束的行
grep "e$" 1.log
```





#### 5 curl命令

```bash
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





#### 6 crontab命令

```bash
crontab -l    # 查看
crontab -e    # 编辑
--------------------------------------------
# 语法
*    *    *    *    *    /bin/ls
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期 (0 - 7) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 天数 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
--------------------------------------------
 
# 每分钟执行一次
*/5 * * * * sh test.sh
```





#### 7 file命令

```bash
find /tmp -name '*.sh'    # 查找/tmp目录下所有后缀为.sh的文件
 
find /tmp -name "[A-Z]*"    # 查找/tmp目录下所有以大写字母开头的文件
 
find /tmp -size 2M    # 查找/tmp 目录下等于2M的文件
find /tmp -size +2M   # 查找/tmp 目录下大于2M的文件
find /tmp -size -2M   # 查找/tmp 目录下小于2M的文件
find /tmp -size +4k -size -5M    # 查找/tmp目录下大于4k，小于5M的文件
 
find /tmp -type f -mtime +7    # 查找/tmp目录下7天前文件
```





#### 8 awk命令

```bash
awk -F ':' '{print $2}' 1.log    # 以冒号作为分隔符，打印第2域内容
awk '{if ($12 > 1000) print $0}' access.log|more    # 使用if判断语句
```





#### 9 sort命令

```bash
grep "ERROR" 1.log|sort -nr    # 按数值倒序排列
```





#### 10 uniq命令

```bash
grep "eims" 1.log|uniq -c    # 去重
```





#### 11 sed命令

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





#### 12 split命令

```bash
split -l 100000 nc.csv -d nc_    # 切割csv文件，切割后文件名为nc_**
rename nc* nc*.csv    # 批量重命名
-----------------------------------------
-l : 按输出文件行数。
-b : 按输出文件大小
-d : 后缀为数字，默认为字母
-a : 默认为2，意思是后缀的位数
-----------------------------------------
```





#### 13 tcpdump命令

```bash
# 监视所有送到主机hostname的数据包
tcpdump -i ens33 dst host 192.168.198.11 >/tmp/1.log
 
# 截获主机hostname发送的所有数据
tcpdump -i ens33 src host 192.168.198.11 >/tmp/1.log
```





#### 14 date命令

```bash
date "+%Y-%m-%d %H:%M:%S"    # 格式化时间
 
time=`date "+%Y-%m-%d %H:%M:%S"`    # shell变量设置
```







