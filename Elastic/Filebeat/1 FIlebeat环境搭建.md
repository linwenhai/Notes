## FIlebeat环境搭建

版本：filebeat 7.2.1

### 1 filebeat.yml

```yml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /app/logs/stdout_*.log
  multiline.pattern: '^[0-9]{4}-[0-9]{2}'
  multiline.negate: true
  multiline.match: after
filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false
setup.template.settings:
  index.number_of_shards: 6
setup.template.enabled: true
setup.template.name: "test"
setup.template.pattern: "test-*"
setup.ilm.enabled: false
output.elasticsearch:
  hosts: ["192.168.100.11:9200"]
  index: "test-%{+yyyy.MM.dd}"
processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~
```

注释：

/app/logs/stdout_*.log：应用日志。

number_of_shards设置需要和number_of_routing_shards成倍数关系。

multiline.pattern: '^[0-9]{4}-[0-9]{2}'：非日期开头的行合并为一行。



### 2 启动

```bash
# 不写服务器日志
./filebeat -e -c test.yml >/dev/null 2>&1 &
 
# 日志输出到logs
./filebeat -c test.yml &
```





