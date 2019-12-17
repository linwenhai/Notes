## Zookeeper



zookeeper节点数为奇数。

```shell
cd /app
tar -zxvf zookeeper-3.4.6.tar.gz
ln -s zookeeper-3.4.6 zookeeper

cd /app/zookeeper/conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg
```

> 修改如下内容
>
> dataDir=/app/zookeeper/data/zookeeper
> server.1=nn01:2888:3888
> server.2=nn02:2888:3888
> server.3=dn01:2888:3888
> server.4=dn02:2888:3888
> server.5=dn03:2888:3888



在每个节点的dataDir目录创建文件myid，id值为数字，如

| 节点 | id值 |
| ---- | ---- |
| nn01 | 1    |
| nn02 | 2    |
| dn01 | 3    |
| dn02 | 4    |
| dn03 | 5    |

```shell
vi /app/zookeeper/zookeeper/myid
```



```shell
#启动ZK
/app/zookeeper/bin/zkServer.sh start

/app/zookeeper/bin/zkServer.sh status

# 测试zk
/app/zookeeper/bin/zkCli.sh
[zk: localhost:2181(CONNECTED) 1] ls /

jps
```





```
ls  /   查找根目录
create /test abc   创建节点并赋值
get /test   获取指定节点的值
set /test cb  设置已存在节点的值
rmr /test  递归删除节点
delete /test/test01  删除不存在子节点的节点
```

