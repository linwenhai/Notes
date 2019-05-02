crontab

#### 1 安装

```shell
rpm -qa|grep vixie-cron
rpm -qa|grep crontabs

/sbin/service crond status		
/sbin/service crond stop
/sbin/service crond start
chkconfig crond --list	
chkconfig crond on	
```



#### 2 cron任务格式

```
minute hour day month dayofweek command
-----------------------------------------
minute:0-59
hour:0-23
day:1-31
month:1-12
week:0-6	/*0表示星期天*/
command：命令或脚本
--周与日月不能同时并存
```



#### 3 crontab命令

```shell
crontab –l			#列出任务
crontab –e			#编辑crontab文件
```

```
例子:
*/2   *   *   *   *       命令		#每两分钟就执行 
0 6,12,18   *   *   *     命令	  	#每天6点、12点、18点执行
```











