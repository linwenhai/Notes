## Shell

`#!`告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

```shell
#!/bin/bash
echo "Hello World !"
```



#### 1 shell变量

```shell
your_name="runoob.com"		#变量名和等号之间不能有空格
echo $your_name				#使用变量
echo ${your_name}			#加花括号是为了帮助解释器识别变量的边界
```



#### 2 字符串

```shell
str='this is a string'
str="this is a string"		#双引号里可以有变量,可以出现转义字符
```



#### 3 数组

```
语法：数组名=(值1 值2 ... 值n)
```

```shell
array_name=(value0 value1 value2 value3)
```

```shell
array_name[0]=a		#单独定义数组的各个分量
array_name[1]=b
array_name[n]=c
```



#### 4 注释

以 `# `开头的行就是注释，会被解释器忽略。

```shell
# 多行注释
:<<EOF	
注释内容...
注释内容...
注释内容...
EOF
```



#### 5 传递参数

脚本内获取参数的格式为：`$n`。

`$0`: 为执行的文件名，`$1`: 第一个参数，`$2`: 第二个参数，当n>=10时，需要使用${n}来获取参数

```shell
#!/bin/bash

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

```
$ sh test.sh 1 2 3
------------------------------------
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2

参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。                     |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。         |
| $-       | 显示Shell使用的当前选项，与set命令功能相同。                 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |





#### 6 运算符

1】关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

| 运算符 | 说明                                                  | 举例                       |
| :----- | :---------------------------------------------------- | :------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |



2】布尔运算符

| 运算符 | 说明                                                | 举例                                     |
| :----- | :-------------------------------------------------- | :--------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                  |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | [ $a -lt 20 -o $b -gt 100 ] 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | [ $a -lt 20 -a $b -gt 100 ] 返回 false。 |



3】逻辑运算符

| 运算符 | 说明       | 举例                                       |
| :----- | :--------- | :----------------------------------------- |
| &&     | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑的 OR  | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true |



4】字符串运算符

| 运算符 | 说明                                      | 举例                     |
| :----- | :---------------------------------------- | :----------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。   | [ $a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否相等，不相等返回 true。 | [ $a != $b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。     | [ -z $a ] 返回 false。   |
| -n     | 检测字符串长度是否为0，不为0返回 true。   | [ -n "$a" ] 返回 true。  |
| $      | 检测字符串是否为空，不为空返回 true。     | [ $a ] 返回 true。       |



7 echo命令



8 printf命令



9 test命令



#### 10 流程控制

1】 if语句

```shell
# 语法
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```



2】for循环

```shell
#语法
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```



3】while语句

```shell
#语法
while condition
do
    command
done
```



#### 11 函数

```shell
#语法
[ function ] funname [()]

{

    action;

    [return int;]

}
```

```shell
#!/bin/bash
# 定义了函数demoFun并进行调用

demoFun(){
    echo "这是我的第一个shell函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

```shell
#!/bin/bash
# 定义一个带有return语句的函数funWithReturn

funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

```shell
#!/bin/bash
# 带参数的函数funWithParam

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```



#### 12 输入输出重定向

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

1】输出重定向

```shell
command1 > file1	#file1内的已经存在的内容将被新内容替代
command1 >> file1	#将新内容添加在文件末尾，不替代
```

2】输入重定向

```shell
command1 < file1	#从文件file1获取输入
```

3】深入重定向

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

```shell
command 2 >> file		# stderr追加到file文件末尾
command >> file 2>&1	# stdout和stderr合并后重定向到file
```

4】Here Document

```shell
语法：
command << delimiter
    document
delimiter
```

```shell
#例子
wc -l << EOF
www.baidu.com
www.163.com
www.runoob.com
EOF
```

5】/dev/null文件

```shell
command > /dev/null			#执行命令，不在屏幕上显示输出结果
command > /dev/null 2>&1	#屏蔽 stdout 和 stderr
```



