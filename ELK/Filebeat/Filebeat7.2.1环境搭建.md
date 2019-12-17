## Filebeat7.2.1环境搭建



### 1 filebeat.yml配置文件

输出到logstash

```yml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /app/nginx/logs/access.log
  fields:
    log_source: next-sfpay-core-nginx-access
  multiline.pattern: '^[0-9]{2}-[0-9]{2}'
  multiline.negate: true
  multiline.match: after    
   
filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false
setup.template.settings:
  index.number_of_shards: 1
output.logstash:
  hosts: ["192.168.1.21:5044","192.168.1.22:5044"]
processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~
```



输出到ES

```YML
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /app/elasticsearch/logs/elk-es.log
  fields:
    log_source: test
  multiline.pattern: '^\['
  multiline.negate: true
  multiline.match: after
  
filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false
setup.template.settings:
  index.number_of_shards: 1
output.elasticsearch:
  hosts: ["http://10.110.39.241:9200"]
  index: "elk-es-%{+yyyy.MM.dd}"
setup.template.name: "elk-es"
setup.template.pattern: "elk-es-*"
setup.ilm.enabled: false
processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~
```



```
multiline.pattern：指定要匹配的正则表达式模式
multiline.negate：定义是否否定模式。默认值为false。
multiline.match：指定Filebeat如何将匹配的行组合到事件中，设置为after或before。

# 不以日期开头(如:2017-02-14)的行都合并到上一行的末尾
multiline.pattern: '^[0-9]{4}-[0-9]{2}-[0-9]{2}'
multiline.negate: true
multiline.match: after

# 不以[开头的行都合并到上一行的末尾
multiline.pattern: '^\['
multiline.negate: true
multiline.match: after 
```



```
# 多个输入路径文件
filebeat.inputs:
- type: log
  paths:
    - /var/log/*.log
    - /var/path2/*.log
```



```
# include_lines包含行，exclude_lines排除行


filebeat.inputs:
- type: log
  ...
  exclude_lines: ['^DBG']

filebeat.inputs:
- type: log
  ...
  include_lines: ['^ERR', '^WARN']
```







### 2 实例启动/关闭

```shell
cd /app/deploy/filebeat/
chmod 755 filebeat
 
# 启动
nohup /app/filebeat/filebeat -e -c /app/filebeat/filebeat.yml &
 
# 关闭
ps -ef|grep filebeat|awk '{print $2}'|xargs kill -9
```



### 3 filebeat监控

默认端口：16900

```shell
ps -ef|grep filebeat
 
netstat -nap|grep PID
```

