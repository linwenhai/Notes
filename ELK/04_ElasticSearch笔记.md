## ElasticSearch



### 1 linux安装

```shell
tar -xvf elasticsearch-7.3.0-linux-x86_64.tar.gz
cd elasticsearch-7.3.0 / bin
./elasticsearch
```





### 2 集群健康

1】集群健康检查

```shell
curl -XGET http://10.119.97.210:9200/_cat/health?v
```

- Green - everything is good (cluster is fully functional)
- Yellow - all data is available but some replicas are not yet allocated (cluster is fully functional)
- Red - some data is not available for whatever reason (cluster is partially functional)



2】获得群集中的节点列表

```shell
curl -XGET http://10.119.97.210:9200/_cat/nodes?v
```



### 3 索引

1】查看集群索引列表

```shell
curl -XGET http://10.119.97.210:9200/_cat/indices?v
```



2】创建索引



### 4 Elasticsearch异常

Elasticsearch索引只读 （index read-only / allow delete (api)）

>日志如下：
>
>logstash.outputs.elasticsearch] retrying failed action with response code: 403 ({"type"=>"cluster_block_exception", "reason"=>"blocked by: [FORBIDDEN/12/index read-only / allow delete (api)]



解决方法：

```shell
curl -XPUT -H "Content-Type: application/json" http://ip:9200/_all/_settings -d '{"index.blocks.read_only_allow_delete": null}'
```





