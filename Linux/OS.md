

```shell
#防火墙
systemctl status firewalld.service
systemctl stop firewalld.service
systemctl disable firewalld.service
```



```shell
#关闭selinux,把SELINUX=enforcing改为SELINUX=disabled即可
vi /etc/selinux/config
```



```shell
#配置阿里源
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum clean all
yum makecache
```



```shell
#本地yum源
vi /etc/yum.repos.d/rhel-source.repo
```

```
[rhel-source]
name=Red Hat Enterprise Linux $releasever - $basearch - Source
baseurl=file:///mnt		
enabled=1
gpgcheck=0
```

```shell
yum clean all
yum list
```





```shell
# 查看物理CPU个数
cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l
```



```shell
# 查看逻辑CPU的个数
cat /proc/cpuinfo| grep "processor"| wc -l
```



```shell
#挂载新盘
fdisk -l
fdisk /dev/sdb
```

```shell
Welcome to fdisk (util-linux 2.23.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xf5d505e3.
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p  
Partition number (1-4, default 1): 1
First sector (2048-125829119, default 2048): 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-125829119, default 125829119): 
Using default value 125829119
Partition 1 of type Linux and of size 60 GiB is set
Command (m for help): w
The partition table has been altered!
Calling ioctl() to re-read partition table.
Syncing disks
```

```shell
mkfs.xfs -f /dev/sdb1
mkdir /app
mount -t xfs /dev/sdb1 /app
vi /etc/fstab
```

```
----------------添加开机启动---------------------------
/dev/sdb1 /app                                  xfs     defaults        0 0
```















