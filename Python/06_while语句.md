## while语句



### 1 input()函数

使用函数input()时，Python将用户输入解读为字符串。

```python
name = input("Please enter your name: ")
print("Hello," + name + "!")
```

```
Please enter your name: lin
输出结果：
Hello,lin!
```



### 2 while循环

```python
c_number = 1		#初始值
while c_number <= 5:
    print(c_number)
    c_number += 1	#计数器
```



```python
x = "\n Tell me something, and I will repeat it back to you:"
x += "\nEnter 'quit' to end program."
message = ""
while message != 'quit':
    message = input(x)
    if message != 'quit':
        print(message)
```





### 3 使用break 退出循环

```python
x = "\n Tell me something, and I will repeat it back to you:"
x += "\nEnter 'quit' to end program."
while True:
    city = input(x)
    if city == 'quit':
        break
    else:
        print(city.title())
```



### 4 在循环中使用continue

执行continue语句，让Python忽略余下的代码，并返回到循环的开头。

```python
cur_number = 0
while cur_number < 10:
    cur_number += 1
    if cur_number % 2 == 0:
        continue
    print(cur_number)
```

```
输出结果：
1
3
5
7
9
```



### 5 在列表之间移动元素

```python
unconf_users = ['lin','xin','zhang']
conf_users = []
while unconf_users:
    conf_user = unconf_users.pop()
    conf_users.append(conf_user)
for conf_user in conf_users:
    print(conf_user.title())
```



### 6 删除包含特定值的所有列表元素

```python
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
print(pets)
while 'cat' in pets:
    pets.remove('cat')
print(pets)
```

```
输出结果：
['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
['dog', 'dog', 'goldfish', 'rabbit']
```











