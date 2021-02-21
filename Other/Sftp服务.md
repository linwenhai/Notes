

#### 1 配置

```bash
groupadd sftp
useradd -g sftp -s /bin/false test    # 创建sftp用户
passwd test
 
mkdir -p /opt/sftp            # 创建用户sftp主目录
usermod -d /opt/sftp test
 
chown root:sftp /opt/sftp  # 文件夹所有者必须为root，用户组可以不是root
chmod 755 /opt/sftp   # 权限不能超过755，否则会导致登录报错，可以是755
 
mkdir /opt/sftp/upload  # 新建上传目录
chown test:sftp /opt/sftp/upload  
chmod 755 /opt/sftp/upload  
 
vi /etc/ssh/sshd_config
-----------注释这行-------------------------
#Subsystem      sftp    /usr/libexec/openssh/sftp-server
-------------------------------------------
```



#### 2 重启SSH

```bash
systemctl restart sshd.service
systemctl status sshd.service
```



#### 3 测试

```bash
sftp test@192.168.100.11
```



