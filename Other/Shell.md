## Shell



#### 1 第一个shell

```bash
#!/bin/bash
echo "Hello Shell"
```



#### 2 变量

变量名和等号之间不能有空格。

```bash
# 定义变量
a="hello"
```



使用一个定义过的变量，只要在变量名前面加美元符号即可。

```bash
# 使用变量
a="hello"
echo $a
```



#### 3 引号

单引号（‘’）：被单引号包裹的内容会将其视为字符串。

双引号（“”）：被双引号包裹内容的转义字符输出。

反引号（``）（键盘数字1左边键）：被小引号包裹的内容会先执行。



#### 4 字符串

```bash
# 字符串拼接
a="Hello"
b="Shell"
echo "$a$b"	
```

>HelloShell



```bash
a="Hello"
echo ${#a}		# 获取字符串长度
echo ${a:1:3}	# 提取子字符串，由索引1开始，3个字符
```



#### 5 数组

```bash
a=(10 20 30)	# 定义数组
echo ${a[1]}	# 获取数组索引为1的元素
echo ${a[@]}	# 获取数组中的所有元素
echo ${#a[@]}	# 获取数组中元素个数
```



#### 6 传递参数

向脚本传递参数，脚本内获取参数的格式为：**$n**。

**n** 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

**$0** 为执行的文件名（包含文件路径）

test.sh

```bash
#!/bin/bash
echo "$0"
echo "$1"
echo "$2"
```

```bash
sh test.sh 10 20
```

>test.sh
>10
>20



#### 7 运算符

假定变量 a 为 10，变量 b 为 20

【1 算术运算符】

| 运算符 | 说明                                          | 举例                          |
| :----- | :-------------------------------------------- | :---------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 -10。   |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 将把变量 b 的值赋给 a。  |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [ $a == $b ] 返回 false。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。      |

**注意：**条件表达式要放在方括号之间，并且要有空格，例如: **[$a==$b]** 是错误的，必须写成 **[ $a == $b ]**。



【2 关系运算符】

关系运算符只支持数字。

| 运算符 | 说明                                                  | 举例                       |
| :----- | :---------------------------------------------------- | :------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |



【3 布尔运算符】

| 运算符 | 说明                                                | 举例                                     |
| :----- | :-------------------------------------------------- | :--------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                  |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | [ $a -lt 20 -o $b -gt 100 ] 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | [ $a -lt 20 -a $b -gt 100 ] 返回 false。 |



【4 逻辑运算符】

| 运算符 | 说明       | 举例                                       |
| :----- | :--------- | :----------------------------------------- |
| &&     | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑的 OR  | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true |



【5 字符串运算符】

| 运算符 | 说明                                         | 举例                     |
| :----- | :------------------------------------------- | :----------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。      | [ $a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否不相等，不相等返回 true。  | [ $a != $b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。        | [ -z $a ] 返回 false。   |
| -n     | 检测字符串长度是否不为 0，不为 0 返回 true。 | [ -n "$a" ] 返回 true。  |
| $      | 检测字符串是否为空，不为空返回 true。        | [ $a ] 返回 true。       |



#### 8 echo命令

echo 指令用于字符串的输出。

```bash
echo "Hello"		# 显示普通字符串
echo "\"Hello\""	# 显示转义字符

name="shell"
echo $name			# 显示变量

echo -e "OK\n"			# 显示换行

echo -e "Hello \c"	# 显示不换行
echo -e "Shell"

echo "Hello Shell" > 1.log	# 显示结果定向至文件

echo '$name'	# 原样输出字符串，不进行转义或取变量

echo `date`		# 显示命令执行结果
```



#### 9 printf命令

printf 命令模仿 C 程序库（library）里的 printf() 程序。

**％s** 输出一个字符串，**％d** 整型输出，**％c** 输出一个字符，**％f** 输出浮点数。

**%-10s** 指一个宽度为 10 个字符（**-** 表示左对齐，没有则表示右对齐），不足则自动以空格填充。

**%-4.2f** 指格式化为浮点数，其中 **.2** 指保留2位小数。



#### 10 流程控制

##### 10.1 if 语句

```bash
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```

```bash
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

```bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```



【示例】

```bash
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```



##### 10.2 for 语句

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

【示例】

```bash
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

>```
>The value is: 1
>The value is: 2
>The value is: 3
>The value is: 4
>The value is: 5
>```



##### 10.3 while 语句

```bash
while condition
do
    command
done
```

【示例】

```bash
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```



无限循环语法格式：

```bash
while true
do
    command
done
```



##### 10.4 until语句

```bash
until condition
do
    command
done
```

condition 一般为条件表达式，如果返回值为 false，则继续执行循环体内的语句，否则跳出循环。

【示例】

```bash
#!/bin/bash
a=0
until [ ! $a -lt 10 ]
do
   echo $a
   a=`expr $a + 1`
done
```



##### 10.5 case语句

```bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

【示例】

```bash
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```



##### 10.6 break 语句

break命令允许跳出所有循环（终止执行后面的所有循环）。

```bash
#!/bin/bash
while true
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```



##### 10.7 continue语句

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。

```bash
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```



#### 11 输入输出

重定向命令列表如下：

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



 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

```bash
command >> file 2>&1		# 将 stdout 和 stderr 合并后重定向到 file
```



如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null。

```bash
 command > /dev/null 2>&1		# 屏蔽 stdout 和 stderr
```



#### 12 read命令

Linux read命令用于从标准输入读取数值。

**参数说明:**

- -a 后跟一个变量，该变量会被认为是个数组，然后给其赋值，默认是以空格为分割符。
- -d 后面跟一个标志符，其实只有其后的第一个字符有用，作为结束的标志。
- -p 后面跟提示信息，即在输入前打印提示信息。
- -e 在输入的时候可以使用命令补全功能。
- -n 后跟一个数字，定义输入文本的长度，很实用。
- -r 屏蔽\，如果没有该选项，则\作为一个转义字符，有的话 \就是个正常的字符了。
- -s 安静模式，在输入字符时不再屏幕上显示，例如login时输入密码。
- -t 后面跟秒数，定义输入字符的等待时间。
- -u 后面跟fd，从文件描述符中读入，该文件描述符可以是exec新开启的。

【示例】

```bash
#!/bin/bash
echo "输入你的名字: "  
read website  
echo "你输入的名字是 $website"  
exit 0 #退出
```

