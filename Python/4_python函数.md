

#### 4.8 异常处理

异常处理使用`try`和`except `关键字。

```PYTHON
try:
    a=input("type a number: ")
    b=input("type another: ")
    a=int(a)
    b=int(b)
    print(a/b)
except (ZeroDivisionError,ValueError):
    print("Invalid input")
```

type a number: lin
type another: wen
Invalid input

type a number: 3
type another: 0
Invalid input

不要在except 语句中使用try 语句定义的变量，会导致新的异常出现。

