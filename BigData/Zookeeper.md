

#### 1 配置

```bash
# zookeeper集群节点数为奇数。
cd /opt
tar -zxvf zookeeper-3.4.6.tar.gz
ln -s zookeeper-3.4.6 zookeeper
 
cd /opt/zookeeper/conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg
 
-------------修改内容-------------------------------
dataDir=/opt/zookeeper/data
server.1=192.168.100.11:2888:3888
server.2=192.168.100.12:2888:3888
server.3=192.168.100.13:2888:3888
---------------------------------------------------
```



```bash
# 192.168.100.11 myid 为1
# 192.168.100.12 myid 为2
# 192.168.100.13 myid 为3
 
echo "1" > /opt/zookeeper/data/myid
```



#### 2 启停

```bash
/opt/zookeeper/bin/zkServer.sh start    # 启动
/opt/zookeeper/bin/zkServer.sh stop     # 停止
/opt/zookeeper/bin/zkServer.sh status   # 查看状态
```



#### 3 命令

```bash
/app/zookeeper/bin/zkCli.sh
[zk: localhost:2181(CONNECTED) 1] ls  /   # 查找根目录
[zk: localhost:2181(CONNECTED) 1] create /test abc   # 创建节点并赋值
[zk: localhost:2181(CONNECTED) 1] get /test   # 获取指定节点的值
[zk: localhost:2181(CONNECTED) 1] set /test cb  # 设置已存在节点的值
[zk: localhost:2181(CONNECTED) 1] rmr /test  # 递归删除节点
[zk: localhost:2181(CONNECTED) 1] delete /test/test01  # 删除不存在子节点的节点
```

