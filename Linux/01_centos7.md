centos 7



#### 1 防火墙

```shell
systemctl status firewalld.service		#查看防火墙状态
systemctl stop firewalld.service		#关闭防火墙
systemctl disable firewalld.service		#禁用防火墙
```



#### 2 selinux

```shell
vi /etc/selinux/config			#关闭selinux
#把SELINUX=enforcing改为SELINUX=disabled即可
```

#### 

#### 3 设置163yum源

http://mirrors.163.com/.help/centos.html

首先备份/etc/yum.repos.d/CentOS-Base.repo

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

下载对应版本repo文件, 放入/etc/yum.repos.d/(操作前请做好相应备份)

- [CentOS7](http://mirrors.163.com/.help/CentOS7-Base-163.repo)
- [CentOS6](http://mirrors.163.com/.help/CentOS6-Base-163.repo)
- [CentOS5](http://mirrors.163.com/.help/CentOS5-Base-163.repo)

运行以下命令生成缓存

```
yum clean all
yum makecache
```



#### 4 配置阿里源

```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

```
yum clean all
yum makecache
```



#### 5  本地yum源

```shell
vi /etc/yum.repos.d/rhel-source.repo
```

```
[rhel-source]
name=Red Hat Enterprise Linux $releasever - $basearch - Source
baseurl=file:///mnt		
enabled=1
gpgcheck=0
```

/mnt为光盘挂载目录

```shell
yum clean all
yum list
yum -y install gcc*		#安装软件
```



#### 6 查看CPU

```shell
cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l 	#查看物理CPU的个数
cat /proc/cpuinfo |grep "processor"|wc -l		#查看逻辑CPU的个数 
cat /proc/cpuinfo |grep "cores"|uniq 			#查看CPU的核数
cat /proc/cpuinfo |grep MHz|uniq 			    #查看CPU的主频
```



#### 7 中文乱码解决方法

```shell
locale -a |grep "zh_CN"			#查看系统是否安装中文语言包 
yum groupinstall "fonts" -y		#没有输出，说明没有安装，输入命令安装
locale -a |grep "zh_CN"			#安装完成，查看安了哪些中文语言包
```

```shell
echo $LANG				#看看当前系统语言环境
vi /etc/locale.conf		#设置中文环境
----------------------
LANG="zh_CN"
```

```shell
source   /etc/locale.conf		#装载配置
echo $LANG			#验证
date				#验证
```





