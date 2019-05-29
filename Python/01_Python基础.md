## Python基础

#### 1 数据类型

Python的数据类型：整数、浮点数、字符串、布尔值、空值

转义字符`\`，`\n`表示换行，`\t`表示制表符，`\\`表示\`。

一个布尔值只有`True`、`False`两种值。



#### 2 变量

```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
```

执行`a = 'ABC'`，解释器创建了字符串`'ABC'`和变量`a`，并把`a`指向`'ABC'`：

![py-var-code-1](assets/0.png)

执行`b = a`，解释器创建了变量`b`，并把`b`指向`a`指向的字符串`'ABC'`：

![py-var-code-2](assets/0.png)

执行`a = 'XYZ'`，解释器创建了字符串'XYZ'，并把`a`的指向改为`'XYZ'`，但`b`并没有更改：

![py-var-code-3](assets/0-1559010147143.png)

所以，最后打印变量`b`的结果自然是`'ABC'`了。



#### 3 常量



#### 4 字符编码

Python 3的字符串使用Unicode，直接支持多语言。

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```



###### 1】格式化

| 占位符 | 替换内容     |
| :----- | :----------- |
| %d     | 整数         |
| %f     | 浮点数       |
| %s     | 字符串       |
| %x     | 十六进制整数 |

```python
print('%02d-%02d' % (3, 27))
print('%.2f' % 3.1415926)
```

```
打印结果：
03-27
3.14
```



#### 5 列表list

list是一种有序的集合，可以随时添加和删除其中的元素。

```python
names = ['Michael', 'Bob', 'Tracy']
print(names)
print(len(names))
print(names[0])			#列表索引是从0开始的
print(names[1])
print(names[-1])		#最后一个元素，用-1做索引
```

```
打印结果：
['Michael', 'Bob', 'Tracy']
3
Michael
Bob
Tracy
```



```python
names = ['Michael', 'Bob', 'Tracy']
names.append('Adam')		#append()方法，末尾追加元素
print(names)
```

```
打印结果：
['Michael', 'Bob', 'Tracy', 'Adam']
```











