### 1 pip换源

**1】windows**

在 *C:\Users\用户名* 目录下创建pip文件夹及文件pip.ini

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

**2】linux**

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



### 2 pip命令

```bash
# 查看python安装模块
pip list

# 安装模块，最新版
pip install numpy

# 安装指定版本
pip install numpy==1.18.5

# 升级模块
pip install -U numpy

# 卸载模块
pip install numpy==1.18.5
```

