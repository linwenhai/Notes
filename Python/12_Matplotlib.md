## Matplotlib

Matplotlib 是一个Python的 2D绘图库。

通过 Matplotlib，可生成绘图，直方图，功率谱，条形图，错误图，散点图等。

### 1 绘图基础

| 方法名                           | 说明                           |
| -------------------------------- | ------------------------------ |
| title()                          | 设置图表的名称                 |
| xlabel()                         | 设置x轴名称                    |
| ylabel()                         | 设置y轴名称                    |
| xticks(x,ticks,rotation)         | 设置x轴的刻度,rotation旋转角度 |
| yticks()                         | 设置y轴的刻度                  |
| plot()                           | 绘制线性图表                   |
| show()                           | 显示图表                       |
| legend()                         | 显示图例                       |
| text(x,y,text)                   | 显示每条数据的值  x,y值的位置  |
| figure(name,figsize=(w,h),dpi=n) | 设置图片大小                   |



### 2 绘制折线图

```python
#coding:utf-8
import matplotlib.pyplot as plt

x=[1,2,3,4,5]
y=[1,4,9,16,25]

plt.plot(x,y,linewidth=5)   # 参数linewidth：线条粗细

plt.rcParams['font.sans-serif'] = ['SimHei']    # 设置中文乱码问题
plt.title("开方图",fontsize=24)    # 标题
plt.xlabel("原始值",fontsize=14)     # X轴，Y轴标签
plt.ylabel("开方值",fontsize=14)
plt.tick_params(axis='both',labelsize=14)   #设置刻度的样式

plt.show()      # 函数show()打开matplotlib查看器
```



```python
"""一元二次方程的曲线"""
import matplotlib.pyplot as plt
x=range(-100,100)
y=[i**2 for i in x]
plt.plot(x,y)
plt.show()
```



```python
"""绘制正弦曲线和余弦曲线"""
import matplotlib.pyplot as plt
import numpy as np
x=np.linspace(0,10,100)
sin_y=np.sin(x)
plt.plot(x,sin_y)
cos_y=np.cos(x)
plt.plot(x,cos_y)
plt.show()
```



```python
"""将画布分为区域，将图画到画布的指定区域"""
import matplotlib.pyplot as plt
import numpy as np
x=np.linspace(1,10,100)
plt.subplot(2,2,1)  # 将画布分为2行2列，将图画到画布的1区域
plt.plot(x,np.sin(x))
plt.subplot(2,2,3)
plt.plot(x,np.cos(x))
plt.show()
```



### 2 绘制散点图

```python
#coding:utf-8

"""绘制散点图例子"""
import matplotlib.pyplot as plt

x=[1,2,3,4,5]
y = [1,4,9,16,25]
plt.scatter(x,y,s=100)

plt.rcParams['font.sans-serif'] = ['SimHei']
plt.title("开方图",fontsize=24)
plt.xlabel("值",fontsize=14)
plt.ylabel("开方值",fontsize=14)
plt.tick_params(axis='both',which='major',labelsize=14)

plt.show()
```



```python
#coding:utf-8

"""绘制散点图例子，使用循环绘制大量点"""
import matplotlib.pyplot as plt

x=list(range(1,1001))
y=[i**2 for i in x]
plt.scatter(x,y,c='red',edgecolors='none',s=10)     #参数c：颜色，参数edgecolors：黑色轮廊默认值none
plt.axis([0,1100,0,1100000])    # 函数axis参数X轴最小最大范围，Y轴最小最大范围

plt.show()
```



```python
#coding:utf-8

"""绘制散点图例子，使用颜色映射"""
import matplotlib.pyplot as plt

x=list(range(1,1001))
y=[i**2 for i in x]
plt.scatter(x,y,c=y,cmap=plt.cm.Blues,edgecolors='none',s=10) # cmap将y值较小设置浅蓝色，较大值设置为深蓝色
plt.axis([0,1100,0,1100000])

plt.show()
```



```python
# 自动保存图表
plt.savefig('squares.png',bbox_inches='tight')
```



```python
"""绘制随机位置、颜色的散点图"""
import matplotlib.pyplot as plt
import numpy as np
np.random.seed(0)
x=np.random.rand(100)
y=np.random.rand(100)
colors=np.random.rand(100)
size=np.random.rand(100)*1000
plt.scatter(x,y,c=colors,s=size,alpha=0.7)
plt.show()
```





