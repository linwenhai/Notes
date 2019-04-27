centos 7



#### 1 查看防火墙状态

systemctl status firewalld.service

#### 2 关闭防火墙

systemctl stop firewalld.service

#### 3 禁用防火墙

systemctl disable firewalld.service

#### 4 关闭selinux

找到/etc/selinux/config 文件，把文件中的SELINUX=enforcing 改为SELINUX=disabled 即可



#### 5 配置阿里源

mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

yum clean all

yum makecache

