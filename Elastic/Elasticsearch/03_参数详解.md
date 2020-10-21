## elasticsearch.yml参数详解

### 1 参数详解

| 参数                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| cluster.name: my-es                                          | 集群名称                                                     |
| node.name: node-1                                            | 当前配置所在机器的节点名                                     |
| node.master: true                                            | 指定该节点是否有资格被选举成为node，默认是true               |
| node.data: true                                              | 指定该节点是否存储索引数据，默认为true                       |
| index.number_of_shards: 5                                    | 索引分片个数，默认为5片                                      |
| index.number_of_replicas: 1                                  | 索引副本个数，默认为1个副本                                  |
| path.conf: /path/to/conf                                     | 文件的存储路径，默认是es根目录下的config文件夹               |
| path.data: /path/to/data                                     | 索引数据的存储路径，默认是es根目录下的data文件夹             |
| path.work: /path/to/work                                     | 临时文件的存储路径，默认是es根目录下的work文件夹             |
| path.logs: /path/to/logs                                     | 日志文件的存储路径，默认是es根目录下的logs文件夹             |
| path.plugins: /path/to/plugins                               | 插件的存放路径，默认是es根目录下的plugins文件夹              |
| bootstrap.memory_lock: true                                  | 锁住内存不进行swapping                                       |
| network.bind_host: 192.168.0.1                               | 绑定的ip地址，可以是ipv4或ipv6的，默认为0.0.0.0              |
| network.publish_host: 192.168.0.1                            | 其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址 |
| network.host: 192.168.0.1                                    | 用来同时设置bind_host和publish_host上面两个参数              |
| transport.tcp.port: 9300                                     | 节点之间交互的tcp端口，默认是9300                            |
| transport.tcp.compress: false                                | 是否压缩tcp传输时的数据，默认为false，不压缩                 |
| http.port: 9200                                              | 对外服务的http端口，默认为9200                               |
| http.max_content_length: 100mb                               | 内容的最大容量，默认100mb                                    |
| http.enabled: true                                           | 是否使用http协议对外提供服务，默认为true                     |
| gateway.type: local                                          | gateway的类型，默认为local即为本地文件系统                   |
| gateway.recover_after_nodes: 1                               | 集群中N个节点启动时进行数据恢复，默认为1                     |
| gateway.recover_after_time: 5m                               | 初始化数据恢复进程的超时时间，默认是5分钟                    |
| gateway.expected_nodes: 2                                    | 集群中N个节点启动，就会立即进行数据恢复，默认为2             |
| cluster.routing.allocation.node_initial_primaries_recoveries: 4 | 初始化数据恢复时，并发恢复线程的个数，默认为4                |
| cluster.routing.allocation.node_concurrent_recoveries: 4     | 添加删除节点或负载均衡时并发恢复线程的个数，默认为4          |
| indices.recovery.max_size_per_sec: 0                         | 数据恢复时限制的带宽，默认为0，即无限制                      |
| indices.recovery.concurrent_streams: 5                       | 恢复数据时最大同时打开并发流的个数，默认为5                  |
| discovery.zen.minimum_master_nodes: 1                        | 保证集群中的节点可以知道其它N个有master资格的节点。默认为1   |
| discovery.zen.ping.timeout: 3s                               | 集群中自动发现其它节点时ping连接超时时间，默认为3秒          |
| discovery.zen.ping.multicast.enabled: true                   | 是否打开多播发现节点，默认是true                             |
| discovery.zen.ping.unicast.hosts: ["172.16.2.141:9300","172.16.2.142:9300","172.16.2.143:9300"] | 集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点 |
| discovery.seed_hosts: ["10.216.22.69", "10.216.22.70", "10.216.22.71"] | es7.x 之后新增的配置，写入候选主节点的设备地址，在开启服务后可以被选为主节点 |
| cluster.initial_master_nodes: ["es-node1", "es-node2", "es-node3"] | es7.x 之后新增的配置，初始化一个新的集群时需要此配置来选举master |
| http.cors.enabled: true                                      | 是否支持跨域，在使用head插件时需要此配置                     |
| http.cors.allow-origin: "*"                                  | “*” 表示支持所有域名                                         |



### 2 ES7 elasticsearch.yml 例子

```xml
cluster.name: elk-es
node.name: node-69
network.host: 10.216.22.69
http.port: 9200
discovery.seed_hosts: ["10.216.22.69", "10.216.22.70", "10.216.22.71"]
cluster.initial_master_nodes: ["node-69", "node-70", "node-71"]
```

