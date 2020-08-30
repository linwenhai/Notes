## Hadoop运维管理

### 1 常用命令

#### 1.1 dfsadmin命令

| 命令                                              | 描述                 |
| ------------------------------------------------- | -------------------- |
| hadoop dfsadmin -report                           | 数据节点状态列表     |
| hdfs dfsadmin -safemode [enter\|get\|leave\|wait] | 把集群切换到安全模式 |
| hadoop dfsadmin -refreshNodes                     | 添加或删除数据节点   |

```shell
# 查看是否进入安全模式
hadoop dfsadmin -safemode get
```

```shell
# 进入安全模式
hadoop dfsadmin -safemode enter
```

```shell
# 退出安全模式
hadoop dfsadmin -safemode leave
```



#### 1.2 fsck命令

fsck检测HDFS文件是否可用。

```shell
hadoop fsck /
```

```shell
Status: HEALTHY
Total size: 3130802412834 B
Total dirs: 143
Total files: 322
Total symlinks: 0
Total blocks (validated): 11824 (avg. block size 264783695 B)
Minimally replicated blocks: 11824 (100.0 %)
Over-replicated blocks: 0 (0.0 %)
Under-replicated blocks: 0 (0.0 %)
Mis-replicated blocks: 0 (0.0 %)
Default replication factor: 3
Average block replication: 3.001184
Corrupt blocks: 0
Missing replicas: 0 (0.0 %)
Number of data-nodes: 14
Number of racks: 2
FSCK ended at Mon Nov 13 11:00:37 CST 2017 in 642 milliseconds
```

参数：

- status：代表这次hdfs上block检测的结果
- Total size: 代表/目录下文件总大小
- Total dirs：代表检测的目录下总共有多少个目录
- Total files：代表检测的目录下总共有多少文件
- Total symlinks：代表检测的目录下有多少个符号连接
- Total blocks(validated)：代表检测的目录下有多少个block块是有效的
- Minimally replicated blocks：代表拷贝的最小block块数
- Over-replicated blocks：指的是副本数大于指定副本数的block数量
- Under-replicated blocks：指的是副本数小于指定副本数的block数量
- Mis-replicated blocks：指丢失的block块数量
- Default replication factor: 3 指默认的副本数是3份（自身一份，需要拷贝两份）
- Missing replicas：丢失的副本数
- Number of data-nodes：有多少个节点
- Number of racks：有多少个机架

```shell
# 查看文件对应的块
hadoop fsck /user/1.txt
```

选项参数，默认都会输出：

- -files：显示文件名称、大小、块数量及是否可用。
- -blocks：显示块在文件中的信息。
- -racks：显示块所在机架位置及datanode位置。

#### 1.3 balancer命令

```shell
start-balancer.sh
```

hdfs-site.xml配置文件参数dfs.balance.bandwidthPerSec属性（balancer占用带宽），默认为1MB。



### 2 hadoop集群备份

```shell
# 2个集群hadoop版本相同
hadoop distcp hdfs://namenode1/foo hdfs://namenode2/
```

```shell
# 2个集群hadoop版本不相同
hadoop distcp hftp://namenode1:50070/foo hdfs://namenode2/
```



### 3 hadoop添加移除节点

#### 3.1 添加新datanode节点

1】修改所有节点hosts文件，添加新节点IP映射；

2】配置namenode免密SSH到新节点；

3】修改namenode配置文件slaves，同步到其余节点；

4】拷贝hadoop目录到新节点；

5】新datanode节点启动进程

```shell
hadoop-daemon.sh start datanode
yarn-daemon.sh start nodemanager
```

6】在主节点上进行刷新

```shell
hdfs dfsadmin -refreshNodes
hdfs dfsadmin -report
```



#### 3.2 移除datanode节点

1】Namenode节点的hdfs-site.xml

```xml
<property>
    <name>dfs.hosts.exclude</name>
    <value>/opt/hadoop/etc/hadoop/conf/excludes</value>
</property>
```

2】Namenode节点的mapred-site.xml

```xml
<property>
   <name>mapred.hosts.exclude</name>
   <value>/opt/hadoop/etc/hadoop/conf/excludes</value>
   <final>true</final>
</property>
```

3】新建excludes文件，文件里写要删除节点datanode；

4】namenode执行

```shell
hadoop mradmin –refreshNodes
hadoop dfsadmin –refreshNodes    # task进程可以kill进程ID
hadoop dfsadmin -report
```

当节点处于Decommissioned，表示关闭成功。