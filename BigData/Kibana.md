## Kibana



#### 一、安装配置

##### 1.1 配置文件

/opt/kibana/config/kibana.yml

```
server.port: 5601
server.host: "192.168.1.11"
elasticsearch.hosts: ["http://192.168.1.100:9200"]
elasticsearch.requestTimeout: 90000
```



/opt/kibana/bin/kibana

```
NODE_OPTIONS="$NODE_OPTIONS --max-old-space-size=4096"
```



##### 1.2 启停脚本

start_kibana.sh

```bash
#! /bin/bash
num=`ps -C node --no-header |wc -l`
if [ $num -eq 0 ]; then
        nohup /opt/kibana/bin/kibana >nohup.out 2>&1 &
fi
```

stop_kibana.sh

```bash
#! /bin/bash
num=`ps -C node --no-header |wc -l`
if [ $num -ne 0 ]; then
        ps -C node --no-header|awk '{print $1}'|xargs kill -9
fi
```



##### 1.3 重启检测脚本

```bash
#! /bin/bash
num=`ps -C node --no-header |wc -l`
if [ $num -eq 0 ]; then
        nohup /opt/kibana/bin/kibana &
fi
```



























