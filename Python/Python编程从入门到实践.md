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









