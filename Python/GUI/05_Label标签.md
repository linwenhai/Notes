## Label 标签

Label（标签） 主要用于显示文本信息， 也可以显示图像 。

Label（标签） 有这样一些常见属性 。

| 属性         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| width,height | 用于指定区域大小                                             |
| font         | 指定字体和字体大小， 如： font = (font_name,size)            |
| image        | 显示在 Label 上的图像， 目前 tkinter 只支持 gif 格式。       |
| fg 和 bg     | fg（foreground） :前景色、 bg（background） :背景色          |
| justify      | 针对多行文字的对齐， 可设置 justify 属性， 可选值"left","center" or "right" |

```python
#测试Label组件的基本用法，使用面向对象的方式
from tkinter import *
class Application(Frame):
    def __init__(self,master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()
 
    def createWidget(self):
        self.label01 = Label(self,text="程序员",width=10,height=2,bg="black",fg="white")
        self.label01["text"] = "CCC"
        self.label01.config(fg="red",bg="green")
        self.label01.pack()
 
        self.label02 = Label(self,text="林一",width=10,height=2,bg="blue",fg="white",font=("黑体",30))
        self.label02.pack()
 
        global  photo
        photo = PhotoImage(file="d:/1.gif")
        self.label03 = Label(self,image=photo)
        self.label03.pack()
 
        self.label04 = Label(self,text="林二",borderwidth=5,relief="groove",justify="right")
        self.label04.pack()
 
if __name__ == '__main__':
    root = Tk()
    root.geometry("400x200+200+300")
    app = Application(master=root)
    root.mainloop()
```

