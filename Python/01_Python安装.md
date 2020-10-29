## Python安装

### 1 windows环境

OS版本：windows 10

Python版本：python3.6.8

```bash
wget https://www.python.org/ftp/python/3.6.8/python-3.6.8-amd64.exe
```

![image-20201028135949415](D:\Notes\Python\image\image-20201028135949415.png)

![image-20201028140016711](D:\Notes\Python\image\image-20201028140016711.png)

![image-20201028140202584](D:\Notes\Python\image\image-20201028140202584.png)





### 2 Linux环境

OS版本：CentOS 7.8

Python版本：Python 3.6.8

```shell
# 安装依赖包
yum install gcc patch libffi-devel python-devel zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y
```

```shell
# 安装python
cd /opt
wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tgz
tar -zxvf Python-3.6.8.tgz
cd Python-3.6.8
./configure --prefix=/opt/python --with-ssl
make && make install
/opt/python/bin/python3 # 验证
```



### 2 Pip配置源

1】windows环境

在 *C:\Users\用户名* 目录下创建pip文件夹及文件pip.ini

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

2】linux环境

```bash
mkdir ~/.pip
vi ~/.pip/pip.conf
```

```bash
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

3】pip命令

```bash
pip list	# 查看python安装模块

pip install numpy	# 安装模块，最新版

pip install numpy==1.18.5	# 安装指定版本

pip install -U numpy	# 升级模块

pip install numpy==1.18.5	# 卸载模块
```



### 3 Pycharm配置

1】配置SSH

打开Pycharm，File—>Settings—>Project—>Project Interpreter 选择Add Remote

![img](D:\Notes\Python\image\20200928102928794.png)

![img](D:\Notes\Python\image\2020092810311593.png)

![img](D:\Notes\Python\image\20200928103322216.png)

![img](D:\Notes\Python\image\20201013155330944.png)

![img](D:\Notes\Python\image\20201013155427640.png)



2】配置Deployment

File -> Settings -> Build, Execution, Deployment -> Deployment

![img](D:\Notes\Python\image\2020092810350966.png)

![img](D:\Notes\Python\image\20200928103606937.png)



3】配置运行环境

![img](D:\Notes\Python\image\20201014113642195.png)