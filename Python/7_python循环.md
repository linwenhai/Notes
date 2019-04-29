循环



#### 7.1 for循环

for 循环：一种用来遍历可迭代对象的循环。

语法：

for [变量名] in [可迭代对象名]: 

​		[指令]

```python
# 使用for循环遍历列表元素
name = ["lin","wen","hai"]
for x in name:
    print(x)

# 使用for循环遍历元组元素
name = ("lin","wen","hai")
for y in name:
    print(y)

# 使用for循环遍历字典元素
name = {"姓":"lin","名":"wen hai"}
for z in name:
    print(z)
```

```python
# 使用for 循环修改可变且可迭代对象中的元素
tv = ["lin","wen","hai"]
i = 0
for show in tv:
    new = tv[i]
    new = new.upper()
    tv[i]=new
    i = i + 1
print(tv)
```

```python
# enumerate 函数去遍历该函数返回的结果
tv = ["lin","hai"]
for i,show in enumerate(tv):
    new = tv[i]
    new = new.upper()
    tv[i] = new
print(tv)
```

