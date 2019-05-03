URL测试

```shell
vi url_test.sh
```

```shell
#!/bin/bash
source /etc/init.d/functions

function usage(){
    echo "usage:$0: url"
    exit 1        
}

function check_url(){
    wget --spider -q -o /dev/null  --tries=1 -T 5 $1
    if [ $? -eq 0 ]
        then
            action "$1 is ok !" /bin/true
    else
            action "$1 is bad !" /bin/false
    fi
}

function main(){
    if [ $# -eq 0 ]
        then
            usage
    fi
    check_url $1
}

main $*
```

```shell
sh url_test.sh www.baidu.com
```

![1556810309437](C:\Users\linwenhai\AppData\Roaming\Typora\typora-user-images\1556810309437.png)

