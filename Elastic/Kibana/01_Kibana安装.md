## Kibana安装



#### 1 Kibana配置

/opt/kibana/config/kibana.yml

1】elasticsearch密码明文配置

```bash
server.port: 5601
server.host: 10.206.231.86
elasticsearch.hosts: ["http://192.168.100.11:9200"]
elasticsearch.username: "elastic"
elasticsearch.password: "111qqq"
i18n.locale: "zh-CN"
```

2】elasticsearch密码密文配置

```bash
bin/kibana-keystore --allow-root create
bin/kibana-keystore --allow-root add elasticsearch.username
bin/kibana-keystore --allow-root add elasticsearch.password
```

```bash
server.port: 5601
server.host: 10.206.231.86
elasticsearch.hosts: ["http://192.168.100.11:9200"]
i18n.locale: "zh-CN"
```





#### 2 启动关闭

```bash
cd /opt/kibana/bin
./kibana > kibana.log 2>&1 &

ps -ef|grep node|grep -v "grep"|awk '{print $2}'|xargs kill -9
```

