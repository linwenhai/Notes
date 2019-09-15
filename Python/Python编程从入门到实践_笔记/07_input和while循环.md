## 用户输入和while循环

### 1 函数input()

函数input()让程序暂停运行，等待用户输入一些文本。

```python
message = input("Tell me something, and I will repeat it back to you: ")
print(message)
```



使用int()来获取数值输入。

```python
>>> age = input("How old are you? ")
How old are you? 21
>>> age = int(age)
>>> age >= 18
True
```



求模运算符%，将两个数相除并返回余数。

```python
>>> 4 % 3
1
>>> 5 % 3
2
```



### 2 while循环

for循环用于针对集合中的每个元素都一个代码块，而while循环不断地运行，直到指定的条件不满足为止。



### 3 使用标志退出循环

定义一个变量，用于判断整个程序是否处于活动状态。这个变量被称为标志。

```python
active = True
while active:
	message = input(prompt)

	if message == 'quit':
		active = False

	else:
		print(message)
```



### 4 使break退出循环

```python
while True:
	city = input(prompt)
	if city == 'quit':
		break
	else:
		print("I'd love to go to " + city.title() + "!")
```



### 5 在循环中使continue

要返回到循环开头，并根据条件测试结果决定是否继续执行循环，可使用continue语句

```python
current_number = 0
while current_number < 10:
	current_number += 1
	if current_number % 2 == 0:
		continue
	print(current_number)
```

>输出结果：
>
>1
>3
>5
>7
>9



### 6 在列表之间移动元素

```python
unconfirmed_users = ['alice', 'brian', 'candace']
confirmed_users = []

while unconfirmed_users:
	current_user = unconfirmed_users.pop()
	print("Verifying user: " + current_user.title())
	confirmed_users.append(current_user)

print("\nThe following users have been confirmed:")
for confirmed_user in confirmed_users:
	print(confirmed_user.title())
```

>输出结果：
>
>Verifying user: Candace
>Verifying user: Brian
>Verifying user: Alice
>
>The following users have been confirmed:
>Candace
>Brian
>Alice



### 7 删除包含特定值的所有列表元素

函数remove()

```python
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
print(pets)
while 'cat' in pets:
	pets.remove('cat')
print(pets)
```

>输出结果：
>
>['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
>['dog', 'dog', 'goldfish', 'rabbit']



### 8 使用用户输入来填充字典

```python
responses = {}
polling_active = True

while polling_active:
	name = input("\nWhat is your name? ")
	response = input("Which mountain would you like to climb someday? ")

	responses[name] = response

	repeat = input("Would you like to let another person respond? (yes/ no) ")
	if repeat == 'no':
		polling_active = False

print("\n--- Poll Results ---")
for name, response in responses.items():
	print(name + " would like to climb " + response + ".")
```

>输出结果：
>
>What is your name? Eric
>Which mountain would you like to climb someday? Denali
>Would you like to let another person respond? (yes/ no) yes
>
>What is your name? Lynn
>Which mountain would you like to climb someday? Devil's Thumb
>Would you like to let another person respond? (yes/ no) no
>
>--- Poll Results ---
>Lynn would like to climb Devil's Thumb.
>Eric would like to climb Denali.

