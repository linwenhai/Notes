



```shell
com1=`date "+%Y-%m-%d %H:%M:%S"`
com2=`curl -o /dev/null -s -w "http://wwww.baidu.com http_code:%{http_code}\n" http://www.baidu.com`
echo $com1 $com2 >>url_check.log
```

