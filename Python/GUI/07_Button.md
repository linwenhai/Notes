## Button

Button（按钮） 用来执行用户的单击操作。 Button 可以包含文本， 也可以包含图像。 按钮被单击后会自动调用对应事件绑定的方法 。

```python
#测试 Button组件的基本用法，使用面向对象的方式
from tkinter import *
from tkinter import messagebox
 
class Application(Frame):
    def __init__(self,master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()
 
    def createWidget(self):
        self.btn01 = Button(root,text="登录",anchor=NE,command=self.login)
        self.btn01.pack()
        global photo
        photo = PhotoImage(file="2.gif")
        self.btn02 = Button(root,image=photo,command=self.login)
        self.btn02.pack()
        self.btn02.config(state="disabled")
 
    def login(self):
        messagebox.showinfo("欢迎你","进入编程世界")
 
if __name__ == '__main__':
    root = Tk()
    root.geometry("400x130+200+300")
    app = Application(master=root)
    root.mainloop()
```

