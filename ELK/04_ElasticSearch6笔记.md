## ElasticSearch

### 1 基本概念

**Cluster**

集群是一个或多个节点（服务器）的集合，它们共同保存您的整个数据，并提供跨所有节点的联合索引和搜索功能。群集有唯一名称标识，默认情况下为“elasticsearch”。



**Node**

节点是作为群集一部分的单个服务器，存储数据并参与群集的索引和搜索功能。节点有名称标识，默认情况下，分配给节点的随机通用唯一标识符（UUID）。



**Index**

索引是具有某些类似特征的文档集合。索引有名称标识（必须全部为小写），并且此名称用于在对其中的文档执行索引，搜索，更新和删除操作时引用索引。



**Type**

已在6.0.0弃用

一种类型，是索引的逻辑类别/分区，允许您在同一索引中存储不同类型的文档。



**Document**

文档是可以编制索引的基本信息单元。文档以JSON（JavaScript Object Notation）表示。



**Shards & Replicas**

分片作用：

- 水平分割/缩放内容量
- 跨分片（可能在多个节点上）分布和并行化操作，从而提高性能/吞吐量



副本作用：

- 在碎片/节点出现故障时提供高可用性。因此，请务必注意，副本分片永远不会在与从中复制的原始/主分片相同的节点上分配。
- 允许您扩展搜索量/吞吐量，因为可以在所有副本上并行执行搜索。



默认情况下，Elasticsearch中的每个索引都分配了5个主分片和1个副本，这意味着如果群集中至少有两个节点，则索引将包含5个主分片和另外5个副本分片（1个完整副本），总计为每个索引10个分片。



每个Elasticsearch分片都是Lucene索引。单个Lucene索引中可以包含最多文档数。截止`LUCENE-5843`，限制是`2,147,483,519`（= Integer.MAX_VALUE - 128）文档。您可以使用`_cat/shards`API 监控分片大小。







### 2 安装

需java8环境

```shell
curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.6.2.tar.gz
tar -xvf elasticsearch-6.6.2.tar.gz
cd elasticsearch-6.6.2/bin
./elasticsearch
```



安装成功，显示如下信息

```sh
[2016-09-16T14:17:51,251][INFO ][o.e.n.Node               ] [] initializing ...
[2016-09-16T14:17:51,329][INFO ][o.e.e.NodeEnvironment    ] [6-bjhwl] using [1] data paths, mounts [[/ (/dev/sda1)]], net usable_space [317.7gb], net total_space [453.6gb], spins? [no], types [ext4]
[2016-09-16T14:17:51,330][INFO ][o.e.e.NodeEnvironment    ] [6-bjhwl] heap size [1.9gb], compressed ordinary object pointers [true]
[2016-09-16T14:17:51,333][INFO ][o.e.n.Node               ] [6-bjhwl] node name [6-bjhwl] derived from node ID; set [node.name] to override
[2016-09-16T14:17:51,334][INFO ][o.e.n.Node               ] [6-bjhwl] version[6.6.2], pid[21261], build[f5daa16/2016-09-16T09:12:24.346Z], OS[Linux/4.4.0-36-generic/amd64], JVM[Oracle Corporation/Java HotSpot(TM) 64-Bit Server VM/1.8.0_60/25.60-b23]
[2016-09-16T14:17:51,967][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [aggs-matrix-stats]
[2016-09-16T14:17:51,967][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [ingest-common]
[2016-09-16T14:17:51,967][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [lang-expression]
[2016-09-16T14:17:51,967][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [lang-mustache]
[2016-09-16T14:17:51,967][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [lang-painless]
[2016-09-16T14:17:51,967][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [percolator]
[2016-09-16T14:17:51,968][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [reindex]
[2016-09-16T14:17:51,968][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [transport-netty3]
[2016-09-16T14:17:51,968][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded module [transport-netty4]
[2016-09-16T14:17:51,968][INFO ][o.e.p.PluginsService     ] [6-bjhwl] loaded plugin [mapper-murmur3]
[2016-09-16T14:17:53,521][INFO ][o.e.n.Node               ] [6-bjhwl] initialized
[2016-09-16T14:17:53,521][INFO ][o.e.n.Node               ] [6-bjhwl] starting ...
[2016-09-16T14:17:53,671][INFO ][o.e.t.TransportService   ] [6-bjhwl] publish_address {192.168.8.112:9300}, bound_addresses {{192.168.8.112:9300}
[2016-09-16T14:17:53,676][WARN ][o.e.b.BootstrapCheck     ] [6-bjhwl] max virtual memory areas vm.max_map_count [65530] likely too low, increase to at least [262144]
[2016-09-16T14:17:56,718][INFO ][o.e.c.s.ClusterService   ] [6-bjhwl] new_master {6-bjhwl}{6-bjhwl4TkajjoD2oEipnQ}{8m3SNKoFR6yQl1I0JUfPig}{192.168.8.112}{192.168.8.112:9300}, reason: zen-disco-elected-as-master ([0] nodes joined)
[2016-09-16T14:17:56,731][INFO ][o.e.h.HttpServer         ] [6-bjhwl] publish_address {192.168.8.112:9200}, bound_addresses {[::1]:9200}, {192.168.8.112:9200}
[2016-09-16T14:17:56,732][INFO ][o.e.g.GatewayService     ] [6-bjhwl] recovered [0] indices into cluster_state
[2016-09-16T14:17:56,748][INFO ][o.e.n.Node               ] [6-bjhwl] started
```



可以看到名为“6-bjhwl”的节点已经启动并在单个集群中选择自己作为主节点。

启动指定集群或节点名称

```sh
./elasticsearch -Ecluster.name=my_es -Enode.name=node1
```





### 3 集群健康

1】检查集群健康

```shell
curl -XGET http://10.119.97.210:9200/_cat/health?v
```

- green：一切都很好（集群功能齐全）
- yellow ：所有数据都可用，但尚未分配一些副本（群集功能齐全）
- red：某些数据由于某种原因不可用（群集部分功能）

 **注意：**当群集为红色时，它将继续提供来自可用分片的搜索请求，但您可能需要尽快修复它，因为存在未分配的分片。



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





