## Window安装

### 1 安装python

python官方下载地址
https://www.python.org/downloads/windows/
选择 executable installer ，根据自己系统选择64位还是32位的安装包。

![image-20200902211506004](.\image\image-20200902211506004.png)

下载完成后双击运行

勾选Add Python 3.7 to PATH，方便在cmd命令行中调用，然后选择Customize installation。

![image-20200902211549841](.\image\image-20200902211549841.png)

pip必选，其他根据自己的情况选择，无Pycharm等python编译器的建议勾选tcl/tk and IDEA，点击Next。

![image-20200902211623804](.\image\image-20200902211623804.png)

上面保持默认，安装目录可以选择安装到系统盘之外的其他盘，点击Install。

![image-20200902211641479](.\image\image-20200902211641479.png)

安装完成。



### 2 pip换源

在 *C:\Users\用户名* 目录下创建pip文件夹；

创建文件pip.ini；

内容：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

阿里云的源：http://mirrors.aliyun.com/pypi/simple/



### 3 pip用法

1】查看已经安装的python库

```
pip list
```

2】安装库numpy

```
pip install numpy
```

3】查看可以升级的包

```
pip list -o
```

4】升级包

```
pip install -U setuptools
```

