## Analyzer

#### 1 什么是 analysis

analysis是 Elasticsearch 在文档发送之前对文档正文执行的过程，以添加到反向索引中（inverted index）。

**Elasticsearch添加文档过程**：

![image-20201029151528352](D:\Notes\Elastic\Elasticsearch\image\image-20201029151528352.png)



analysis有三个部分组成：Char Filters, Tokenizer 及 Token Filter。

- **Char Filter**：字符过滤器的工作是执行清除任务，例如剥离 HTML 标记，还有上面的把 “&” 转换为 “and” 字符串。
- **Tokenizer**：下一步是将文本拆分为称为标记的术语。 这是由 tokenizer 完成的。 可以基于任何规则（例如空格）来完成拆分。 
- **Token filter**：一旦创建了 token，它们就会被传递给 token filter，这些过滤器会对token进行规范化。 Token filter 可以更改 token，删除术语或向 token 添加术语。



一个 analyzer 有且只有一个 tokenizer，有0个或一个以上的 char filter 及 token filter。



**standard analyzer** 是 Elasticsearch 的缺省分析器：

- 没有 Char Filter
- 使用 standard tokonizer
- 把字符串变为小写，同时有选择地删除一些 stop words 等。默认的情况下 stop words 为 _none_，也即不过滤任何 stop words。



**analyzer 场景**：

- 在 indexing 的时候，也即在建立索引的时候
- 在 searching 的时候，也即在搜索时，分析需要搜索的词语

![img](D:\Notes\Elastic\Elasticsearch\image\20190924172304548.png)



**analyzer例子**：

![img](D:\Notes\Elastic\Elasticsearch\image\20190902213526816.png)





#### 2 _analyze API

```bash
# 使用 standard analyzer分析文本
GET /_analyze
{
  "analyzer": "standard",
  "text": "Quick Brown Foxes!"
}
```



#### 3 IK中文分词器

IK中文分词器，包含ik_smart和ik_max_word

下载对应版本：https://github.com/medcl/elasticsearch-analysis-ik/releases

```bash
mkdir -p /opt/elasticsearch/plugins/ik
cd /opt/elasticsearch/plugins/ik
# 上传elasticsearch-analysis-ik-7.2.1.zip
unzip elasticsearch-analysis-ik-7.2.1.zip
```

重启elasticsearch，加载ik插件



```json
POST /_analyze
{
  "text": "我爱北京天安门",
  "analyzer": "ik_max_word"
}
```













