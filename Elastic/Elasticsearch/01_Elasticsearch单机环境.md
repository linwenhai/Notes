## Elasticsearch单机环境

OS：CentOS Linux release 7.6
Elasticsearch：elasticsearch-7.2.1
JDK：elasticsearch7自带JDK，无需配置



### 1 OS设置

```shell
vi /etc/security/limits.conf
---------------添加内容-----------------------------
*               soft    nofile          65536
*               hard    nofile          65536
*               soft    nproc           4096
*               hard    nproc           4096
```

```shell
# 验证，需要退出重新登录
ulimit -Hn
ulimit -Sn
ulimit -Hu
ulimit -Su
```



```shell
vi /etc/sysctl.conf
---------------添加内容-----------------------------
vm.max_map_count=262144
```

```shell
# 验证
sysctl -p
```



### 2 elasticsearch.yml设置

```shell
vi config/elasticsearch.yml
```

```
cluster.name: my-es
node.name: node-1
network.host: 192.168.198.11
http.port: 9200
discovery.seed_hosts: ["192.168.198.11"]
cluster.initial_master_nodes: ["node-1"]
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
xpack.security.transport.ssl.verification_mode: certificate 
xpack.security.transport.ssl.keystore.path: elastic-certificates.p12 
xpack.security.transport.ssl.truststore.path: elastic-certificates.p12
```



### 3 启动ES

```shell
#生成证书
bin/elasticsearch-certutil ca -out config/elastic-certificates.p12 -pass ""

#启动ES
bin/elasticsearch -d

#设置密码
bin/elasticsearch-setup-passwords interactive
```

