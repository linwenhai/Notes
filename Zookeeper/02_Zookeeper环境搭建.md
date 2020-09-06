## Zookeeper环境搭建

### 1 安装zk

```shell
# zookeeper集群节点数为奇数。
cd /app
tar -zxvf zookeeper-3.4.6.tar.gz
ln -s zookeeper-3.4.6 zookeeper

cd /app/zookeeper/conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg
```

>#修改如下内容
>dataDir=/app/zookeeper/data
>server.1=nn01:2888:3888
>server.2=nn02:2888:3888
>server.3=dn01:2888:3888
>server.4=dn02:2888:3888
>server.5=dn03:2888:3888

```shell
#在每个节点的dataDir目录创建文件myid，id值为数字，各节点不一样
echo "1" > /app/zookeeper/zookeeper/myid
```

### 2 启停

```shell
#启动ZK
/app/zookeeper/bin/zkServer.sh start
/app/zookeeper/bin/zkServer.sh status
jsp

# 测试zk
/app/zookeeper/bin/zkCli.sh
[zk: localhost:2181(CONNECTED) 1] ls /
```

### 3 ZK命令

```shell
ls  /   #查找根目录
create /test abc   #创建节点并赋值
get /test   #获取指定节点的值
set /test cb  #设置已存在节点的值
rmr /test  #递归删除节点
delete /test/test01  #删除不存在子节点的节点
```

