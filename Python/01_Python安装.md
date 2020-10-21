OS版本：CentOS Linux release 7.8.2003

Python版本：Python-3.6.9.tgz



### 1 安装依赖包

```shell
# 安装依赖包
yum install gcc patch libffi-devel python-devel zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y
```



### 2 安装python

```shell
cd /opt
wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tgz
tar -zxvf Python-3.6.9.tgz
cd Python-3.6.9
./configure --prefix=/opt/python --with-ssl
make && make install
```



### 3 升级OS python版本

entos7默认python版本2.7

```bash
cd /usr/bin/
rm -rf python python2
ln -s python2.7 python2
ln -s /opt/python/bin/python3 /usr/bin/python
ln -s /opt/python/bin/pip3 /usr/bin/pip
```

```bash
# 修改yum python版本为2
vi /usr/bin/yum
vi /usr/libexec/urlgrabber-ext-down
```

