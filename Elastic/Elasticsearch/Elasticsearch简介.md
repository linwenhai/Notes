## Elasticsearch简介

 Elasticsearch是一个基于[Apache Lucene](http://lucene.apache.org/)(TM)的开源搜索引擎 。 使用Java编写并使用Lucene来建立索引并实现搜索功能，目的是通过简单连贯的RESTful API让全文搜索变得简单并隐藏Lucene的复杂性。 

 **Elasticsearch功能**：

- 分布式的实时文件存储，每个字段都被索引并可被搜索
- 实时分析的分布式搜索引擎
- 可以扩展到上百台服务器，处理PB级结构化或非结构化数据

**Elasticsearch的特点**：

- 高速（speed）：相比较其它的一些大数据引擎，Elasticsearch可以实现秒级的搜索，但是对于它们来说，可能需要数小时才能完成。
- 极易扩展（scale)：Elasticsearch的cluster是一种分布式的部署，很容易使它处理petabytes的数据库容量。
- 搜索结果（relevance)：Elasticsearch搜索的结果可以按照分数进行排序，它能提供我们最相关的搜索结果。



#### 1 检查集群

```shell
$ curl -XGET 'http://localhost:9200/' -H 'Content-Type: application/json'
```



#### 2 索引文档

```shell
curl -XPUT 'http://localhost:9200/twitter/_doc/1?pretty' -H 'Content-Type: application/json' -d '
{
    "user": "kimchy",
    "post_date": "2009-11-15T13:12:00",
    "message": "Trying out Elasticsearch, so far so good?"
}'
 
curl -XPUT 'http://localhost:9200/twitter/_doc/2?pretty' -H 'Content-Type: application/json' -d '
{
    "user": "kimchy",
    "post_date": "2009-11-15T14:12:12",
    "message": "Another tweet, will it be indexed?"
}'
 
curl -XPUT 'http://localhost:9200/twitter/_doc/3?pretty' -H 'Content-Type: application/json' -d '
{
    "user": "elastic",
    "post_date": "2010-01-15T01:46:38",
    "message": "Building the site, should be kewl"
}'
```



#### 3 搜索文档

```shell
# 根据索引ID搜索文档
curl -XGET 'http://localhost:9200/twitter/_doc/1?pretty=true'
curl -XGET 'http://localhost:9200/twitter/_doc/2?pretty=true'
curl -XGET 'http://localhost:9200/twitter/_doc/3?pretty=true'
```



```shell
# 根据内容搜索文档
curl -XGET 'http://localhost:9200/twitter/_search?q=user:kimchy&pretty=true'
```



```shell
# 使用JSON查询语言，根据内容搜索文档
curl -XGET 'http://localhost:9200/twitter/_search?pretty=true' -H 'Content-Type: application/json' -d '
{
    "query" : {
        "match" : { "user": "kimchy" }
    }
}'
```



```shell
# 使用JSON查询语言，根据范围搜索文档
curl -XGET 'http://localhost:9200/twitter/_search?pretty=true' -H 'Content-Type: application/json' -d '
{
    "query" : {
        "range" : {
            "post_date" : { "from" : "2009-11-15T13:00:00", "to" : "2009-11-15T14:00:00" }
        }
    }
}'
```



#### 4 分片

```shell
# 每个索引默认的1个副本1分片，更改为每个索引1个副本的2个分片
curl -XPUT http://localhost:9200/another_user?pretty -H 'Content-Type: application/json' -d '
{
    "settings" : {
        "index.number_of_shards" : 2,
        "index.number_of_replicas" : 1
    }
}'
```

