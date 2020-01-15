## Logstash7.2.1环境搭建



### 1 logstash.conf配置文件

```shell
vi /app/logstash/config/logstash.conf
```

```conf
input {
  beats {
    port => 5044
  }
}
filter {
  if [fields][log_source]=="nginx-access"{
    grok {
      match => {
        "message" => '%{IP:clientip}\s*%{DATA}\s*%{DATA}\s*\[%{HTTPDATE:requesttime}\]\s*"%{WORD:requesttype}.*?"\s*%{NUMBER:status:int}\s*%{NUMBER:bytes_read:int}\s*"%{DATA:requesturl}"\s*%{QS:ua}'
     }
      overwrite => ["message"]
    }
  }
  if [fields][log_source]=="nginx-error"{
    grok {
      match => {
        "message" => '(?<time>.*?)\s*\[%{LOGLEVEL:loglevel}\]\s*%{DATA}:\s*%{DATA:errorinfo},\s*%{WORD}:\s*%{IP:clientip},\s*%{WORD}:%{DATA:server},\s*%{WORD}:\s*%{QS:request},\s*%{WORD}:\s*%{QS:upstream},\s*%{WORD}:\s*"%{IP:hostip}",\s*%{WORD}:\s*%{QS:referrer}'
      }
      overwrite => ["message"]
    }
  }
}
output {
  if [fields][log_source]=="nginx-access"{
    elasticsearch {
      hosts => ["http://192.168.1.31:9200","http://192.168.1.32:9200","http://192.168.1.33:9200"]
      action => "index"
      index => "%{[fields][log_source]}-%{+YYYY.MM.dd}"
   }
  }
  if [fields][log_source]=="nginx-error"{
    elasticsearch {
      hosts => ["http://192.168.1.31:9200","http://192.168.1.32:9200","http://192.168.1.33:9200"]
      action => "index"
      index => "%{[fields][log_source]}-%{+YYYY.MM.dd}"
   }
  }
}

```



### 2 实例启动关闭

```shell
# --config.test_and_exit选项会解析您的配置文件并报告任何错误
/app/logstash/bin/logstash -f kms_kafka.conf --config.test_and_exit
 
# --config.reload.automatic选项启用自动配置重新加载
/app/logstash/bin/logstash -f kms_kafka.conf --config.reload.automatic
 
# 启动
nohup /app/logstash/bin/logstash -f /app/logstash/config/logstash.conf >/dev/null 2>&1 &
# 停止
ps -ef|grep logstash|awk '{print $2}'|xargs kill -9
 
 
# 同一OS启动多个实例，需指定--path.data
nohup /app/logstash/bin/logstash -f /app/logstash/conf/esg-eip-core-5051.conf --path.data=/app/logstash/data/5051 >/dev/null 2>&1 &
```



