## GUI 程序4 个核心步骤 

#### 1 主窗口对象

```python
#创建应用程序主窗口对象（也称： 根窗口）  
#通过类Tk的无参构造函数
from tkinter import *
root = Tk()
```



#### 2 可视化组件 

```python
#在主窗口中，添加各种可视化组件，比如：按钮（Button）、文本框（Label）等。
btn01 = Button(root)
btn01["text"] = "点我送花"
```



#### 3 几何布局管理器

```python
#通过几何布局管理器，管理组件的大小和位置 
btn01.pack()
```



#### 4 事件处理 

```python
#通过绑定事件处理程序， 响应用户操作所触发的事件（比如： 单击、 双击等）
def songhua(e):
	messagebox.showinfo("Message","送你一朵玫瑰花，请你爱上我")
	print("送你99朵玫瑰花")
    
btn01.bind("<Button-1>",songhua)
```



#### 5 例子

```python
from tkinter import  *
from tkinter import messagebox
 
root = Tk()
 
btn01 = Button(root)
btn01["text"] = "点我送花"
 
btn01.pack()
 
def songhua(e):
    messagebox.showinfo("Message","送你一朵玫瑰花")
    print("送你 99 朵玫瑰花")
 
btn01.bind("<Button-1>",songhua)
 
root.mainloop()
```



#### 6 tkinter主窗口

通过 geometry(‘wxh± x± y’)进行设置。 w 为宽度， h 为高度。+x 表示距屏幕左边的距离； -x 表示距屏幕右边的距离； +y表示距屏幕上边的距离； -y 表示距屏幕下边的距离。  

```python
#测试 tkinter 主窗口位置和大小的设置
from tkinter import *
root = Tk()

root.title("测试")
root.geometry("500x400+100+200")    #宽度500，高度400；距屏幕左边100，距屏幕上边200

root.mainloop()
```

