## Python编程从入门到实践



### 5 if语句

#### 5.1 if-else 语句

```python
cars = ['audi', 'bmw', 'subaru', 'toyota']
for car in cars:
    if car == 'bmw':
        print(car.upper())
    else:
        print(car.title())
```

```
打印结果：
Audi
BMW
Subaru
Toyota
```



在Python中检查字符串是否相等时区分大小写。

```
upper()：转换为大写
lower()：转换为小写
title()：首字母转为大写
```



#### 5.2 if-elif-else 语句

```python
age = 14
if age < 4:
    price = 0
elif age >=4 and age < 18:
    price = 5
else:
    price = 10
print("Your admission cost is $" + str(price)+".")
```



#### 5.3 使用if 语句处理列表

```python
names = ['lin','wen','hai']
for name in names:
    if name == 'wen':
        print("Sorry,wen is not here")
    else:
        print("Hello " + name)
```

```
打印结果：
Hello lin
Sorry,wen is not here
Hello hai
```





### 6 字典

在Python中，字典是一系列键—值对，使用花括号{}。

键和值之间用冒号分隔，而键—值对之间用逗号分隔。

alien_0 = {'color': 'green', 'points': 5}



#### 6.1 访问字典中的值

```python
alien_0 = {'color':'green'}
print(alien_0['color'])	
```



#### 6.2 添加键—值对

```python
alien_0 = {'color':'green','points':5}
alien_0['x_position'] = 12
alien_0['y_position'] = 28
print(alien_0)
```



使用字典来存储用户提供的数据或在编写能自动生成大量键—值对的代码时，通常都需要先定义一个空字典。

```python
alien_0 = {}
alien_0['color'] = 'green'
alien_0['points'] = 5
print(alien_0)
```



#### 6.3 要修改字典中的值

可依次指定字典名、用方括号括起的键以及与该键相关联的新值。

```python
alien_0 = {'color':'grenn'}
alien_0['color'] = 'yelllow'
print(alien_0)
```



#### 6.4 删除键—值对

使用del语句时，必须指定字典名和要删除的键。

```python
alien_0 = {'color':'green','points':5}
del alien_0['points']
print(alien_0)
```





















