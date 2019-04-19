## 1 sed

 **sed命令的语法格式**：

sed的命令格式： sed [option] 'sed command'filename

sed的脚本格式：sed [option] -f 'sed script'filename

**sed命令的选项(option)**：

`-n` ：只打印模式匹配的行

`-e` ：直接在命令行模式上进行sed动作编辑，此为默认选项

`-f `：将sed的动作写在一个文件内，用–f filename 执行filename内的sed动作

`-r `：支持扩展表达式

`-i` ：直接修改文件内容



```shell
$ sed -n '2p' error.log		#打印第2行
$ sed -n '1,3p' error.log	#打印第1到第3行
$ sed -n '/second/p' error.log		#打印日匹配sencond字符的行
$ sed -n '/2019\/04\/19 03:50:55/p' error.log
$ sed -n '/03:29:59/,/03:57:16/p' error.log		#sed -n '/起始时间/,/结束时间/p' 日志文件
```

`^` 匹配行开始，如：/^sed/匹配所有以sed开头的行。
`$` 匹配行结束，如：/sed$/匹配所有以sed结尾的行。
`[]` 匹配一个指定范围内的字符。
`[^]` 匹配一个不在指定范围内的字符。

`\ `  转义字符



## 2 awk

**语法**：awk '{pattern + action}' {filenames}

--pattern 表示 AWK 在数据中查找的内容

--action 是在找到匹配内容时所执行的一系列命令



```shell
$ cat /etc/passwd |awk -F ':' '{print $1}'

```

`$0`则表示所有域,`$1`表示第一个域,`$n`表示第n个域。
`-F`指定域分隔符为':'，不指定默认每行按空格或TAB分割。

