

#### 1获取ES

Linux: [elasticsearch-7.2.1-linux-x86_64.tar.gz

```sh
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.2.1-linux-x86_64.tar.gz
```



#### 2 启动ES

```shell
tar -xvf elasticsearch-7.2.1-linux-x86_64.tar.gz

# 启动单节点
cd elasticsearch-7.2.1/bin
./elasticsearch

# 启动第2，3个节点
./elasticsearch -Epath.data=data2 -Epath.logs=log2
./elasticsearch -Epath.data=data3 -Epath.logs=log3
```



#### 3 集群监控检查

```shell
# ES health API
curl -X GET "localhost:9200/_cat/health?v&pretty"
```



4 






