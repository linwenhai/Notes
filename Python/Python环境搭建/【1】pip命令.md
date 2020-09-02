### 1 pip换源

在 *C:\Users\用户名* 目录下创建pip文件夹；

创建文件pip.ini；

内容：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

阿里云的源：http://mirrors.aliyun.com/pypi/simple/



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



