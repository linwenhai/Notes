URL测试

```shell
vi url_test.sh
```

```shell
#!/bin/bash
source /etc/init.d/functions

# 下面的函数实现的是友好型提示，即如果命令输入错误，
# 将会提示此命令的正确用法
function usage(){
    echo "usage:$0: url"
    exit 1        
}

# 函数实现Url检测，如果正常返回url is ok!否则返回 url is bad
function check_url(){
    wget --spider -q -o /dev/null  --tries=1 -T 5 $1
    if [ $? -eq 0 ]
        then
            action "$1 is ok !" /bin/true
    else
            action "$1 is bad !" /bin/false
    fi
}

# 将函数接入方法入口
function main(){
    if [ $# -eq 0 ]
        then
            usage
    fi
    check_url $1
}

# 调用执行Main方法
main $*
```

```shell
sh url_test.sh www.baidu.com
```

![1556810309437](C:\Users\linwenhai\AppData\Roaming\Typora\typora-user-images\1556810309437.png)



```
参数说明： 
$# 返回传入命令的参数个数 
$1返回传入的第一个参数 
$2返回传入的第二个参数 
$*返回传入的所有参数
action系统自带的功能实现，true为OK ，false为failed 
```



