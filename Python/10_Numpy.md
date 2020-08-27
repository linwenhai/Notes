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

**1】numpy.random.random()**

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



**3】numpy.random.randn()**

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
| shape       | 返回数组的维度(n,m)，n 行m 列                              |
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



#### 2.5 其他方式创建

**1】numpy.zeros()**

zeros 创建指定大小的数组，数组元素以0来填充。

```python
import numpy
a=numpy.zeros(5,dtype=int)
b=numpy.zeros((2,3),dtype=int)
print(a)
print(b)
```

>执行结果：
>
>[0 0 0 0 0]
>[[0 0 0]
> [0 0 0]]



**2】numpy.ones()**

numpy.ones 创建指定形状的数组，数组元素以1来填充。

```python
import numpy
a=numpy.ones(5,dtype=int)
b=numpy.ones((2,3),dtype=int)
print(a)
print(b)
```

>执行结果：
>
>[1 1 1 1 1]
>[[1 1 1]
> [1 1 1]]



**3】numpy.empty()**

numpy.empty 方法用来创建一个指定形状（shape）、数据类型（dtype）且未初始化的数组，里面的元素的值是之前内存的值。

```python
import numpy
a=numpy.empty([2,3],dtype=int)
print(a)
```



**4】numpy.linspace()**

linspace 函数用于创建一个一维数组，数组是一个等差数列构成的。

**语法**：numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)

| linspace参数 | 描述                                                       |
| ------------ | ---------------------------------------------------------- |
| start        | 序列的起始值                                               |
| stop         | 序列的终止值                                               |
| num          | 要生成的等步长的样本数量，默认为50                         |
| endpoint     | 该值为ture 时，数列中中包含stop 值，反之不包含，默认是True |
| retstep      | 如果为True 时，生成的数组中会显示间距，反之不显示          |

```python
import numpy
a=numpy.linspace(1,10,10,dtype=int)
b=numpy.linspace(1,10,6,dtype=int)
print(a)
print(b)
```

>执行结果：
>
>[ 1  2  3  4  5  6  7  8  9 10]
>[ 1  2  4  6  8 10]



**5】numpy.logspace()**

numpy.logspace 函数用于创建一个于等比数列。

**语法**：np.logspace(start, stop, num=50, endpoint=True, base=10.0, dtype=None)

| logspace参数 | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| start        | 序列的起始值为：base**start                                  |
| stop         | 序列的终止值                                                 |
| num          | 要生成的等步长的样本数量，默认为50                           |
| endpoint     | 该值为ture 时，数列中中包含stop 值，反之不包含，默认是True。 |
| base         | 对数log 的底数                                               |
| dtype        | ndarray 的数据类型                                           |

```python
import numpy
a=numpy.logspace(1,10,8,base=2,dtype=int)
print(a)
```

>执行结果：
>
>[   2    4   11   28   70  172  420 1024]



### 3 数组的切片和索引

ndarray 对象的内容可以通过索引或切片来访问和修改，与Python 中list 的切片操作一样。

**1】一维数组切片和索引**

```python
import numpy
a=numpy.arange(10)
print(a[2:7:2])
print(a[2:])
```

>执行结果：
>
>[2 4 6]
>[2 3 4 5 6 7 8 9]



**2】根据索引获取元素**

reshape()是数组array中的方法，作用是将数据重新组织。

```python
import numpy
a=numpy.arange(1,13)
b=a.reshape(4,3)
print(b)
print(b[1])         # 获取第2行
print(b[2][1])      # 获取第3行第2列
```

>执行结果：
>
>[[ 1  2  3]
> [ 4  5  6]
> [ 7  8  9]
> [10 11 12]]
>[4 5 6]
>8



**3】二维数组切片**

```python
import numpy
a=numpy.arange(1,13).reshape(4,3)
print(a)
print(a[:,1])       # 所有行的第二列
print(a[::2,0])     # 奇数行的第一列
```

>执行结果：
>
>[[ 1  2  3]
> [ 4  5  6]
> [ 7  8  9]
> [10 11 12]]
>[ 2  5  8 11]
>[1 7]



**4】使用坐标获取数组[x,y]**

```python
import numpy
a=numpy.arange(1,13).reshape(4,3)
print(a)
print(a[2,1])           # 获取第3行第2列
print(a[(2,3),(1,0)])   # 获取第3行第2列，第4行第1列
```

>执行结果：
>
>[[ 1  2  3]
> [ 4  5  6]
> [ 7  8  9]
> [10 11 12]]
>8
>[ 8 10]



**5】索引为负数来获取**

```python
import numpy
a=numpy.arange(1,13).reshape(4,3)
print(a[-1])        # 获取最后1行
print(a[::-1])      # 行进行倒序
print(a[::-1,::-1]) # 行列都倒序
```

>执行结果：
>[10 11 12]
>
>[[10 11 12]
> [ 7  8  9]
> [ 4  5  6]
> [ 1  2  3]]
>
>[[12 11 10]
> [ 9  8  7]
> [ 6  5  4]
> [ 3  2  1]]



### 4 改变数组的维度

| 方法      | 描述                             |
| --------- | -------------------------------- |
| reshape() | 一维数组转二维、三维或者多维数组 |
| ravel()   | 三维数组转一维                   |
| flatten() | 二维数组转一维                   |

```python
import numpy
a=numpy.arange(24).reshape(3,8)     # 一维数组转二维
b=numpy.arange(24).reshape(2,3,4)   # 一维数组转三维
print(a)
print(b)
c=a.flatten()     # 二维数组转一维
d=b.ravel()       # 三维数组转一维
print(c)
print(d)
```



### 5 数组的拼接

#### 5.1 hstack()

hstack 函数可以将两个或多个数组水平组合起来形成一个数组。

```python
import numpy
a=numpy.arange(1,7).reshape(2,3)
b=numpy.arange(11,17).reshape(2,3)

print(numpy.hstack([a,b]))      # 水平堆叠来生成数组
```

>执行结果：
>
>[[ 1  2  3 11 12 13]
> [ 4  5  6 14 15 16]]



#### 5.2 vstack()

stack 函数可以将两个或多个数组垂直组合起来形成一个数组。

```python
import numpy
a=numpy.arange(1,7).reshape(2,3)
b=numpy.arange(11,17).reshape(2,3)

print(numpy.vstack([a,b]))      # 垂直堆叠来生成数组
```

>执行结果：
>
>[[ 1  2  3]
> [ 4  5  6]
> [11 12 13]
> [14 15 16]]



#### 5.3 concatenate()

```python
import numpy
a=numpy.arange(1,7).reshape(2,3)
b=numpy.arange(11,17).reshape(2,3)

print(numpy.concatenate([a,b],axis=0))     # 垂直方向拼接相当于vstack,axis默认为0
print(numpy.concatenate([a,b],axis=1))     # 水平方向拼接相当于hstack
```



### 6 数组的分隔

#### 6.1 hsplit()

hsplit 函数可以水平分隔数组，该函数有两个参数，第1 个参数表示待分隔的数组，第2 个参数表示要将数组水平分隔成几个小数组。

```python
import numpy
a=numpy.arange(1,13).reshape(2,6)
print(numpy.hsplit(a,2))    #纵向切分，分隔成2个小数组
```

>执行结果：
>
>[array([[1, 2, 3],
>       [7, 8, 9]]), array([[ 4,  5,  6],
>       [10, 11, 12]])]



#### 6.2 vsplit()

vsplit 函数可以垂直分隔数组，该函数有两个参数，第1 个参数表示待分隔的数组，第2 个参数表示将数组垂直分隔成几个小数组。

```python
import numpy
a=numpy.arange(1,13).reshape(2,6)
print(numpy.vsplit(a,2))    #横向切分，分隔成2个小数组
```

>执行结果：
>
>[array([[1, 2, 3, 4, 5, 6]]), array([[ 7,  8,  9, 10, 11, 12]])]



#### 6.3 split()

numpy.split 函数沿特定的轴将数组分割为子数组。

语法：numpy.split(ary, indices_or_sections, axis)

| split参数           | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| arv                 | 被分割的数组                                                 |
| indices_or_sections | 如是一个整数，就用该数平均切分，如果是一个数组，为沿轴切分的位置 |
| axis                | 沿着哪个维度进行切向，默认为0，横向切分。为1 ，纵向切分。    |

```python
import numpy
a=numpy.arange(1,13).reshape(2,6)
print(numpy.split(a,2,axis=0))      # 横向切分
print("----------------------")
print(numpy.split(a,2,axis=1))      # 纵向切分
```

>执行结果：
>
>[array([[1, 2, 3, 4, 5, 6]]), array([[ 7,  8,  9, 10, 11, 12]])]
>[array([[1, 2, 3],
>       [7, 8, 9]]), array([[ 4,  5,  6],
>       [10, 11, 12]])]



#### 6.4 transpose()

```python
import numpy
a=numpy.arange(12).reshape(2,6)
print(a.transpose())        # 二维转置
```

>执行结果：
>
>[[ 0  6]
> [ 1  7]
> [ 2  8]
> [ 3  9]
> [ 4 10]
> [ 5 11]]



### 7 算术函数

| 函数        | 描述 |
| ----------- | ---- |
| add()       | +    |
| substract() | -    |
| multiply()  | *    |
| divide()    | /    |



### 8 数学函数

| 函数     | 描述                     |
| -------- | ------------------------ |
| sin()    | 正弦                     |
| cos()    | 余弦                     |
| tan()    | 正切                     |
| around() | 返回指定数字的四舍五入值 |
| floor()  | 返回数字的下舍整数       |
| ceil()   | 返回数字的上入整数       |



### 9 聚合函数

| 函数     | 描述          |
| -------- | ------------- |
| sum()    | 求和          |
| prod()   | 所有元素相乘  |
| mean()   | 平均值        |
| std()    | 标准差        |
| var()    | 方差          |
| median() | 中数          |
| power()  | 幂运算        |
| sqrt()   | 开方          |
| min()    | 最小值        |
| max()    | 最大值        |
| argmin() | 最小值下标    |
| argmax() | 最大值下标    |
| inf      | 无穷大        |
| exp(10)  | 以e为底的指数 |
| log(10)  | 对数          |

