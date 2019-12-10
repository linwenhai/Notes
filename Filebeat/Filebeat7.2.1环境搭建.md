## Filebeat7.2.1环境搭建



### 1 filebeat.yml配置文件

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



不以日期开头(如:2017-02-14)的行都合并到上一行的末尾
multiline.pattern: '^[0-9]{4}-[0-9]{2}-[0-9]{2}'
multiline.negate: true
multiline.match: after

不以[开头的行都合并到上一行的末尾
multiline.pattern: '^\['
multiline.negate: true
multiline.match: after 

当文件中有转义字符时，如果想输出时保留原样，则 sed 语句中要多加几个 ‘\’

```shell
sed -i "s#'\^\[0-9\]{2}-\[0-9\]{2}'#'^\\\\['#" 1.txt
sed -i "s#'\^\[0-9\]{2}-\[0-9\]{2}'#'^[0-9]{4}-[0-9]{2}-[0-9]{2}'#" 1.txt
```



### 2 实例启动/关闭

```shell
cd /app/deploy/filebeat/
chmod 755 filebeat
 
# 启动
nohup ./filebeat -e -c filebeat.yml >/dev/null 2>&1 &
 
# 关闭
ps -ef|grep filebeat|awk '{print $2}'|xargs kill -9
```



### 3 filebeat监控

默认端口：16900

```shell
ps -ef|grep filebeat
 
netstat -nap|grep PID
```

