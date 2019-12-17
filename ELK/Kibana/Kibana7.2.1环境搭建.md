## Kibana7.2.1环境搭建



OS：CentOS release 6.9



### 1 kibana.yml配置文件

```shell
vi /app/elasticsearch/config/kibana.yml
```

```yml
server.port: 5601
server.host: "192.168.1.11"
elasticsearch.hosts: ["http://192.168.1.11:9200"]
elasticsearch.requestTimeout: 90000
```



### 2 配置elasticsearch负载节点

```shell
vi /app/elasticsearch/config/elasticsearch.yml
```

```yml
cluster.name: elk-es
node.name: node-00
network.host: 192.168.1.11
discovery.seed_hosts: ["192.168.1.31", "192.168.1.32", "192.168.1.33"]
cluster.initial_master_nodes: ["node-01","node-02","node-03"]
bootstrap.system_call_filter: false
node.master: false
node.data: false
```



### 3 实例启动关闭

```shell
cd /app/kibana/bin/
 
# 启动
nohup ./kibana 2>>kibana.log &
 
# 停止
ps -ef|grep kibana|awk '{print $2}'|xargs kill -9
```

