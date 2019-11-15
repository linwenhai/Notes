## Logstash

[TOC]







### 1 安装Logstash

Logstash需要Java 8.不支持Java 9



```shell
tar -zxvf jdk-8u201-linux-x64.tar.gz
tar -zxvf logstash-6.6.0.tar.gz
ln -s jdk1.8.0_201 jdk
ln -s logstash-6.6.0 logstash
```



### 2 测试Logstash

```shell
bin/logstash -e 'input { stdin {} } output { stdout {} }'
```

在命令行下输入"hello world"

```
hello world
{
       "message" => "hello world",
    "@timestamp" => 2019-07-15T06:51:45.028Z,
          "host" => "CNSZ17VLK6148",
      "@version" => "1"
}
```



**测试filebeat-->logstash**

**1】filebeat配置**

filebeat.yml

```yaml
filebeat.prospectors:
- type: log
  paths:
    - /path/to/file/logstash-tutorial.log 
output.logstash:
  hosts: ["localhost:5044"]
```

>/path/to/file/logstash-tutorial.log 		----测试日志



```shell
sudo ./filebeat -e -c filebeat.yml -d "publish"
```

>-d publish`显示所有与`publish`相关的信息



**2】logstash配置**

```conf
input {
    beats {
        port => "5044"
    }
}
 filter {
    grok {
        match => { "message" => "%{COMBINEDAPACHELOG}"}
    }
    geoip {
        source => "clientip"
    }
}
output {
    stdout { codec => rubydebug }
}
```

>beats { port => "5044" }的意思是用Beats输入插件，而stdout { codec => rubydebug }的意思是输出到控制台）



```shell
bin/logstash -f first-pipeline.conf --config.test_and_exit
```

```shell
bin/logstash -f first-pipeline.conf --config.reload.automatic
```

>`--config.test_and_exit`选项会解析您的配置文件并报告任何错误。
>
>`--config.reload.automatic`选项启用自动配置重新加载





### 3 input

- **file**：从文件系统上的文件读取，与UNIX命令非常相似 `tail -0F`
- **syslog**：在已知端口514上侦听syslog消息并根据RFC3164格式进行解析
- **redis**：使用redis通道和redis列表从redis服务器读取。Redis通常用作集中式Logstash安装中的“代理”，该安装将Logstash事件从远程Logstash“托运人”排队。
- **beats**：处理[ Beats](https://www.elastic.co/downloads/beats)发送的事件。



**1】当前界面输入**

```ruby
input { stdin { } }
```



**2】 filebeat输入**

```json
input {
    beats {
        port => "5044"
    }
}
```





### 4 filter

- **grok**：解析并构造任意文本。Grok是目前Logstash中将非结构化日志数据解析为结构化和可查询内容的最佳方式。有了内置于Logstash的120种模式，您很可能会找到满足您需求的模式！
- **mutate**：对事件字段执行常规转换。您可以重命名，删除，替换和修改事件中的字段。
- **drop**：完全删除事件，例如*调试*事件。
- **clone**：制作事件的副本，可能添加或删除字段。
- **geoip**：添加有关IP地址的地理位置的信息（也在Kibana中显示惊人的图表！）



**1】grok插件**

grok模式的语法：%{SYNTAX:SEMANTIC}

SYNTAX：代表匹配值的类型,例如3.44可以用NUMBER类型所匹配,127.0.0.1可以使用IP类型匹配。 
SEMANTIC：代表存储该值的一个变量名称。

`语法`指的就是匹配的模式，例如使用 NUMBER 模式可以匹配出数字，IP 则会匹配出 127.0.0.1 IP 地址。

```python
%{NUMBER:lasttime}%{IP:client}
```

默认情况下，所有“语义”都被保存成字符串，你也可以添加转换到的数据类型，目前转换类型只支持 int 和 float



例子：

```
55.3.244.1 GET /index.html 15824 0.043
```

```yml
filter {
	grok {
		match => { "message" => "%{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes} %{NUMBER:duration}" }
	}
}
```

```
输出结果：
client: 55.3.244.1
method: GET
request: /index.html
bytes: 15824
duration: 0.043
```



**2】Geoip插件**

示例：

```yml
filter {
    geoip {
        source => "message"
    }
}
```







**3】date插件**

日期过滤器用于解析字段中的日期，然后使用该日期或时间戳作为事件的logstash时间戳。

```ruby
filter {
  date {
    match => [ "logdate", "MMM dd yyyy HH:mm:ss" ]
  }
}
```









```ruby
filter {
  if [path] =~ "access" {
    mutate { replace => { type => "apache_access" } }
    grok {
      match => { "message" => "%{COMBINEDAPACHELOG}" }
    }
    date {
      match => [ "timestamp" , "dd/MMM/yyyy:HH:mm:ss Z" ]
    }
  } else if [path] =~ "error" {
    mutate { replace => { type => "apache_error" } }
  } else {
    mutate { replace => { type => "random_logs" } }
  }
}
```













### 5 output

- **elasticsearch**：将事件数据发送到Elasticsearch。如果您计划以高效，方便且易于查询的格式保存数据...... Elasticsearch是您的最佳选择。期。是的，我们有偏见:)
- **file**：将事件数据写入磁盘上的文件。
- **graphite**：将事件数据发送到graphite，这是一种用于存储和绘制指标的流行开源工具。
- **statsd**：将事件数据发送到statsd，这是一种“侦听统计信息，如计数器和定时器，通过UDP发送并将聚合发送到一个或多个可插入后端服务”的服务。如果您已经在使用statsd，这可能对您有用！



```json
output {
    elasticsearch {
        hosts => ["IP Address 1:port1", "IP Address 2:port2", "IP Address 3"]
    }
}
```





### 6 logstash.yml配置文件

pipeline.workers			----主机的CPU核心数

pipeline.batch.size		----在尝试执行其过滤器和输出之前，单个工作线程将从输入收集的最大事件数,代价是增加了内存开销

pipeline.batch.delay	----创建管道事件批处理时，在将小型批处理分派给管道工作者之前等待每个事件的时间以毫秒为单位。

































