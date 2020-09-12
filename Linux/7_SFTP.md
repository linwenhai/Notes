## Centos7搭建SFTP

### 1 创建sftp组和sftp用户

```bash
groupadd sftp
cat /etc/group
```

```bash
useradd -g sftp -s /bin/false test01 
passwd test01
```



### 2 新建sftp用户的主目录

```bash
mkdir -p /opt/sftp
usermod -d /opt/sftp test01
```



### 3 sshd_config

```bash
vi /etc/ssh/sshd_config
```

>将如下这行用#符号注释掉
>
>#Subsystem      sftp    /usr/libexec/openssh/sftp-server
>
>Subsystem       sftp    internal-sftp  
>Match Group sftp  
>ChrootDirectory /sftp/%u    
>ForceCommand    internal-sftp    
>AllowTcpForwarding no   
>X11Forwarding no  



### 4 设置Chroot目录权限

```bash
chown root:sftp /opt/sftp  #文件夹所有者必须为root，用户组可以不是root
chmod 755 /opt/sftp   #权限不能超过755，否则会导致登录报错，可以是755
```



### 5 新建上传文件

```bash
mkdir /opt/sftp/upload  
chown test01:sftp /opt/sftp/upload  
chmod 755 /opt/sftp/upload  
```



### 6 重启sshd服务

```bash
systemctl restart sshd.service
systemctl status sshd.service
```



### 7 测试

```bash
sftp test01@127.0.0.1
```

