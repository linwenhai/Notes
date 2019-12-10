## 分片状态UNASSIGNED解决方法



### 1 查询集群是否有UNASSIGNED状态分片

```shell
curl -XGET "http://192.168.1.31:9200/_cat/health?v"

curl -XGET "http://192.168.1.31:9200/_cat/shards?h=index,shard,prirep,state,unassigned.reason"|grep UNASSIGNED
```





```shell
# 查询集群状态，是否有unassigned_shards分片
GET /_cluster/health


# 查询具体异常分片
GET /_cat/shards?h=index,shard,prirep,state,unassigned.reason


# 查询异常索引分片情况
GET /test-2019.12.05/_settings


# 调整number_of_replicas为0
PUT test-2019.12.05/_settings
{
  "index":{
    "number_of_replicas":0
  }
}


# 查询分片情况
GET _cat/shards/test-2019.12.05*


# 对段进行合并
POST /test-2019.12.05/_forcemerge?max_num_segments=1
 
 
# 重新调整number_of_replicas为1
PUT test-2019.12.05/_settings
{
  "index":{
    "number_of_replicas":1
  }
}


# 等待分片由状态INITIALIZING恢复为STARTED
GET _cat/shards/test-2019.12.05*
```

