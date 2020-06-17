



#### 1 filebeat.yml

```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /opt/nginx/logs/access.log
  tags: ["nginx-access"]
  
filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false
setup.template.settings:
  index.number_of_shards: 1
processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~

output.elasticsearch:
  hosts: ["http://192.168.198.11:9200"]
  index: "nginx-access-%{+yyyy.MM.dd}"
  username: "elastic"
  password: "111qqq"
setup.template.name: "nginx-access"
setup.template.pattern: "nginx-access-*"
setup.ilm.enabled: false
```



  multiline.pattern: '^[0-9]{4}-[0-9]{2}'
  multiline.negate: true
  multiline.match: afte





#### 2 启动/关闭

```shell
#启动
nohup /opt/filebeat/filebeat -e -c /opt/filebeat/filebeat.yml >/dev/null 2>&1 &
```

```shell
#关闭
ps -ef|grep filebeat|awk '{print $2}'|xargs kill -9
```









