## Grafana Stable安装



### 一、Grafana Stable安装

使用清华镜像源安装。

#### 1 配置清华镜像源

```bash
# 新建 grafana.repo
vi /etc/yum.repos.d/grafana.repo
```

>```
>[grafana]
>name=grafana
>baseurl=https://mirrors.tuna.tsinghua.edu.cn/grafana/yum/rpm
>repo_gpgcheck=0
>enabled=1
>gpgcheck=0
>```

```bash
yum clean all
yum makecache
```

#### 2 安装Grafana

```bash
yum install grafana
```

#### 3 启动

```bash
service grafana-server start
```

浏览器访问：http://ip:3000

初始密码：admin/admin

![img](D:\Notes\Other\Grafana\image\2117311-20201110102426293-1772001580.png)



