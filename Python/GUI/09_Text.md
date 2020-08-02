## Text

Text(多行文本框)的主要用于显示多行文本， 还可以显示网页链接, 图片, HTML 页面, 甚至 CSS 样式表， 添加组件等。 因此， 也常被当做简单的文本处理器、 文本编辑器或者网页浏览器来使用。 比如 IDLE 就是 Text 组件构成的。  

```python
#测试 Text 多行文本框组件的基本用法， 使用面向对象的方式
from tkinter import *
import webbrowser
 
class Application(Frame):
    def __init__(self,master=None):
        super().__init__(master)    # super()代表的是父类的定义， 而不是父类对象
        self.master =master
        self.pack()
        self.createWidget()
 
    def createWidget(self):
        self.w1 = Text(root,width=40,height=12,bg="gray")
        self.w1.pack()
        self.w1.insert(1.0,"123456789\nabc")
        self.w1.insert(2.3,"努力学习")
        Button(self,text="重复插入文本",command=self.insertText).pack(side="left")
        Button(self,text="返回文本",command=self.returnText).pack(side="left")
        Button(self,text="添加图片",command=self.addImage).pack(side="left")
        Button(self,text="添加组件",command=self.addWidget).pack(side="left")
        Button(self,text="通过tag精确控制文本",command=self.testTag).pack(side="left")
 
    def insertText(self):
        self.w1.insert(INSERT,'linyi')
        self.w1.insert(END,'[shenlan]')
        self.w1.insert(1.8,"linsan")
 
    def returnText(self):
        print(self.w1.get(1.2,1.6))
        print("全部文本：\n"+self.w1.get(1.0,END))
 
    def addImage(self):
        self.photo = PhotoImage(file="2.gif")
        self.w1.image_create(END,image=self.photo)
 
    def addWidget(self):
        b1 = Button(self.w1,text="深蓝")
        self.w1.window_create(INSERT,window=b1)
 
    def testTag(self):
        self.w1.delete(1.0,END)
        self.w1.insert(INSERT,"good good study day up\n深蓝\n程序员\n百度,百度一下")
        self.w1.tag_add("good",1.0,1.9)
        self.w1.tag_config("good",background="yellow",foreground="red")
        self.w1.tag_add("baidu",4.0,4.2)
        self.w1.tag_config("baidu",underline=True)
        self.w1.tag_bind("baidu","<Button-1>",self.webshow)
 
    def webshow(self,event):
        webbrowser.open("http://www.baidu.com")
 
if __name__=='__main__':
    root = Tk()
    root.geometry("400x130+200+300")
    app = Application(master=root)
    root.mainloop()
```



Tags 通常用于改变 Text 组件中内容的样式和功能。 你可以修改文本的字体、 尺寸和颜色。 另外， Tags 还允许你将文本、 嵌入的组件和图片与鼠标和键盘等事件相关联。  

