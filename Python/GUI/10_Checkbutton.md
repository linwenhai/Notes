## Checkbutton

Checkbutton 控件用于选择多个按钮的情况。 Checkbutton可以显示文本， 也可以显示图像。  

```python
#测试 Checkbutton 组件的基本用法， 使用面向对象的方式
from tkinter import *
from tkinter import messagebox
 
class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)    # super()代表的是父类的定义， 而不是父类对象
        self.master =master
        self.pack()
        self.createWidget()
 
    def createWidget(self):
        self.codeHobby = IntVar()
        self.videoHobby = IntVar()
        print(self.codeHobby.get())
        self.c1 = Checkbutton(self, text="编程", variable=self.codeHobby, onvalue=1, offvalue=0)
        self.c2 = Checkbutton(self, text="娱乐", variable=self.videoHobby, onvalue=1, offvalue=0)
        self.c1.pack(side="left");self.c2.pack(side="left")
        Button(self, text="确定", command=self.confirm).pack(side="left")
 
    def confirm(self):
        if self.videoHobby.get()==1:
            messagebox.showinfo("测试","看电视剧")
        if self.codeHobby.get()==1:
            messagebox.showinfo("测试","写代码挣钱")
 
if __name__=='__main__':
    root = Tk()
    root.geometry("400x50+200+300")
    app = Application(master=root)
    root.mainloop()

```

