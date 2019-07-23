## ElasticSearch





```shell
# elasticsearch集群健康检查
curl -XGET http://10.119.97.210:9200/_cat/health?v
curl -XGET http://10.119.97.210:9200/_cluster/health?pretty

# 查询所有节点列表
curl -XGET http://10.119.97.210:9200/_cat/nodes?v

# 查询所有索引
curl -XGET http://10.119.97.210:9200/_cat/indices?v
```

