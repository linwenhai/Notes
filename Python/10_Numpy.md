## Numpy

### 1 安装Numpy

```shell
pip install numpy
```

```python
# 测试numpy
import numpy as np
a=np.arange(10)
print(a)
print(type(a))
```



### 2 创建数组

#### 2.1 array创建

numpy 模块的array 函数可以生成多维数组。

**语法：**

```python
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)
```

| array参数 | 描述                                                       |
| --------- | ---------------------------------------------------------- |
| object    | 数组或嵌套的数列                                           |
| dtype     | 数组元素的数据类型，可选                                   |
| copy      | 对象是否需要复制，可选                                     |
| order     | 创建数组的样式，C 为行方向，F 为列方向，A 为任意方向(默认) |
| subok     | 默认返回一个与基类类型一致的数组                           |
| ndmin     | 指定生成数组的最小维度                                     |

**1】array创建一维数组**

```python
import numpy
a=numpy.array([1,2,3,4,5,6])
print(a)
print('数组a的维度：',a.shape)
```

>执行结果：
>
>[1 2 3 4 5 6]
>数组a的维度： (6,)



**2】array创建二维数组**

```python
import numpy
a=numpy.array([[1,2,3],[4,5,6]])
print(a)
```

>执行结果：
>
>[[1 2 3]
> [4 5 6]]



**3】ndmin参数的使用**

```python
import numpy
a=numpy.array([1,2,3,4,5,6],ndmin=3)
print(a)
```

>执行结果：
>
>[[[1 2 3 4 5 6]]]



**4】dtype参数的使用**

```python
import numpy
a=numpy.array([1,2,3,4,5,6],dtype=float)
print(a)
```

>执行结果：
>
>[1. 2. 3. 4. 5. 6.]



#### 2.2 arange创建

arange 函数创建数值范围并回ndarray对象。

**语法：**

```python
numpy.arange(start, stop, step, dtype)
```

| arange参数 | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| start      | 起始值，默认为0                                              |
| stop       | 终止值（不包含）                                             |
| step       | 步长，默认为1                                                |
| dtype      | 返回ndarray 的数据类型，如果没有提供，则会使用输入数据的类型 |

**1】arange生成一维数组**

```python
import numpy
a=numpy.arange(10,20,2,dtype=int)
print(a)
```

>执行结果：
>
>[10 12 14 16 18]



**2】arange生成二维数组**

```python
import numpy
a=numpy.array([numpy.arange(1,4),numpy.arange(4,7)])
print(a)
```

>执行结果：
>
>[[1 2 3]
> [4 5 6]]



#### 2.3 随机数创建

**1】numpy.random.random(size=None)**

该方法返回[0.0, 1.0)范围的随机数

```python
import numpy
a=numpy.random.random(size=4)
b=numpy.random.random(size=(2,3))
print(a)
print(b)
```

>执行结果：
>
>[0.85893005 0.58543184 0.83870428 0.65511177]
>[[0.49796334 0.04882835 0.48735603]
> [0.47441539 0.86197391 0.07501194]]



**2】 numpy.random.randint()**

该方法有三个参数low、high、size 三个参数。默认high 是None,如果只有low，那范围就是[0,low)。如果有high，范围就是[low,high)，返回随机整数。

```python
import numpy
a=numpy.random.randint(5,10,size=8)
b=numpy.random.randint(5,10,size=(2,3))
print(a)
print(b)
```

>执行结果：
>
>[9 8 9 9 5 8 5 8]
>[[5 9 6]
> [8 7 6]]



**3】numpy.random.randn(d0,d1,…,dn)**

randn 函数返回具有标准正态分布（期望为0，方差为1）数组。

```python
import numpy
a=numpy.random.randn(2,3)
b=numpy.random.randn(2,3,4)
print(a)
print(b)
```

>执行结果：
>
>[[ 0.14555272 -1.37635289 -1.116959  ]
> [ 0.86398647  1.01145049  0.49746098]]
>
>[[[-0.46078327  0.4954248   0.09997309  0.11513032]
>  [ 0.49386392  1.65865093 -0.69593253  0.60454919]
>  [-0.54333701 -3.06795879  0.39511257 -1.1080593 ]]
> [[ 1.757302    0.15103505 -0.06536249  0.93217986]
>  [-1.25541441  0.07199278 -0.1315961  -0.89310062]
>  [-0.11179396  0.51497651  0.31387728  0.52563762]]]



**4】numpy.random.normal()**

normal函数，正太分布（高斯分布）loc：期望scale：方差size 形状

```python
import numpy
a=numpy.random.normal(loc=3,scale=4,size=(2,3))
print(a)
```

>执行结果：
>
>[[-3.48873784  2.72683914  2.10720546]
> [ 6.30130744 -0.38095039  3.24471141]]



#### 2.4 ndarray对象

| ndarray属性 | 描述                                                       |
| ----------- | ---------------------------------------------------------- |
| ndmin       | 轴的数量或维度的数量(n,)                                   |
| shape       | 返回数组的维度(n,y)，n 行m 列                              |
| size        | 数组元素的总个数，相当于.shape 中n*m 的值                  |
| dtype       | ndarray 对象的元素类型                                     |
| itemsize    | ndarray 对象中每个元素的大小，以字节为单位，一个字节是8 位 |

```python
import numpy
a=numpy.random.randint(1,10,size=6)
b=numpy.random.randint(1,10,size=(2,3))
print("a.ndmin: {0}".format(a.ndim))
print("b.ndmin: {0}".format(b.ndim))
print("a.shape: {0}".format(a.shape))
print("b.shape: {0}".format(b.shape))
print("a.dtype: {0}".format(a.dtype))
print("b.dtype: {0}".format(b.dtype))
print("a.size: {0}".format(a.size))
print("b.size: {0}".format(b.size))
print("a.itemsize: {0}".format(a.itemsize))
print("b.itemsize: {0}".format(b.itemsize))
```

>执行结果：
>
>a.ndmin: 1
>b.ndmin: 2
>a.shape: (6,)
>b.shape: (2, 3)
>a.dtype: int32
>b.dtype: int32
>a.size: 6
>b.size: 6
>a.itemsize: 4
>b.itemsize: 4



### 3 数组的切片和索引



### 4 改变数组的维度



### 5 数组的拼接



### 6 数组的分隔



### 7 算术函数



### 8 数学函数



### 9 聚合函数

