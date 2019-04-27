字符串



#### 6.2 索引

字符串中第一个字符所在的索引为0，其后每个索引递增1。使用索引-1 可以查找可迭代对象中的最后一个元素。





#### 6.6 改变大小写

upper		将字符串中的每个字符改为大写
lower		 将字符串中的每个字符改为小写
capitalize	  将字符串的首字母改为大写

```python
>>> x="hai"
>>> "lin wen {}".format(x)
'lin wen hai'
>>> "lin".upper()
'LIN'
>>> "LIN".lower()
'lin'
>>> "lin".capitalize()
'Lin'
```



#### 6.7 格式化

`format` 方法会把字符串中的“{}”替换为传入的字符串

```python
>>> x="hai"
>>> "lin wen {}".format(x)
'lin wen hai'
```



#### 6.8 分割

`split` 传入一个字符串作为split 方法的参数，并用其将原字符串分割为多个字符串

```python
>>> x="lin wen hai,xin ling ling"
>>> x.split(",")
['lin wen hai', 'xin ling ling']
```

#### 

#### 6.9 连接

`join` 方法可以在字符串的每个字符间添加新字符

```python
>>> x="abc"
>>> y="+".join(x)
>>> y
'a+b+c'
```

```python
>>> x=["lin","wen","hai"]
>>> y="".join(x)
>>> y
'linwenhai'
```

```python
>>> x=["lin","wen","hai"]
>>> z=" ".join(x)
>>> z
'lin wen hai'
```



#### 6.10 去除空格

`strip` 方法去除字符串开头和末尾的空白字符。

```python
>>> x=" the "
>>> y=x.strip()
>>> y
'the'
```



#### 6.11 替换

replace 方法中，第一个参数是要被替换的字符串，第二个参数是用来替换的字符串。



