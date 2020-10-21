## Kibana安装



### 1 kibana.yml

```xml
server.port: 5601
server.host: "192.168.198.11"
elasticsearch.hosts: ["http://192.168.198.11:9200"]
elasticsearch.username: "elastic"
elasticsearch.password: "111qqq"
```



### 2 启动关闭

```bash
cd /opt/kibana/bin/
 
# 启动
./kibana 2>>kibana.log &
 
# 停止
ps -ef|grep node|grep -v "grep"|awk '{print $2}'|xargs kill -9
```

