## GUI

常用的 GUI 库 ：Tkinter、wxPython、PyQT  

#### 1 tkinter

基于 tkinter 模块创建 GUI 程序包含如下 4 个核心步骤：  

1】创建应用程序主窗口对象（也称： 根窗口）  

```python
#通过类 Tk 的无参构造函数
from tkinter import *
root = Tk()
```

2】在主窗口中， 添加各种可视化组件， 比如： 按钮（Button） 、文本框（Label） 等  

```python
btn01 = Button(root)
btn01["text"] = "点我就送花"
```

3】通过几何布局管理器， 管理组件的大小和位置  

```python
btn01.pack()
```

4】事件处理，通过绑定事件处理程序， 响应用户操作所触发的事件 ，比如： 单击、 双击等）  



