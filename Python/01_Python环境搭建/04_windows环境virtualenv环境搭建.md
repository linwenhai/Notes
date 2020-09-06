## windows环境virtualenv环境搭建

VirtualEnv可以方便的解决不同项目对类库的依赖问题。



### 1 安装virtualenv

```shell
pip install virtualenv
```



### 2 为项目安装虚拟环境

创建项目目录，如：D:\Python\django_project

```shell
# 安装虚拟环境env
d:
cd D:\Python\django_project
virtualenv env
```



### 3 启动虚拟环境

```shell
# 进入目录，运行activate
cd D:\Python\django_project\env\Scripts
activate
```

![image-20200902221805446](.\image\image-20200902221805446.png)



```shell
# 在虚拟环境安装类库
pip install flask
```



### 4 测试虚拟环境应用

在目录D:\Python\django_project创建fd.py

```python
from flask import Flask
 
app = Flask(__name__)
 
@app.route('/')
def hello_flask():
	return 'hello flask!'
 
if __name__ == '__main__':
	app.run()
```



在虚拟环境中执行该应用

![image-20200902222239602](.\image\image-20200902222239602.png)



在浏览器访问：http://127.0.0.1:5000/



### 5 退出虚拟环境

退出虚拟环境，使用deactivate命令。

![image-20200902222436958](.\image\image-20200902222436958.png)

