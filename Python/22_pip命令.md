### 1 pip换源

**1】windows**

在 *C:\Users\用户名* 目录下创建pip文件夹及文件pip.ini

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

**2】linux**

```shell
vi ~/.config/.pip
```

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

>常用的国内源：
>
>阿里云 http://mirrors.aliyun.com/pypi/simple/
>中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
>豆瓣(douban) http://pypi.douban.com/simple/
>清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
>中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/



### 2 pip命令

#### 2.1 查看已经安装模块

```shell
pip list
```



#### 2.2 安装模块

```shell
# 安装最新版
pip install numpy
```

```shell
# 安装指定版本
pip install numpy==1.18.5
```



#### 2.3 查看可以升级的模块

```shell
pip list -o
```



#### 2.4 升级模块

```shell
pip install -U numpy
```



#### 2.5 卸载模块

```shell
pip install numpy==1.18.5
```



