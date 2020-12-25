## Logstash环境搭建

### 1 jvm.options

```bash
-Xms10g
-Xmx10g
```



### 2 logstash.yml

```bash
pipeline.workers: 2
pipeline.batch.size: 3000
pipeline.batch.delay: 10
```



### 3 测试性能

1】测试logstash输入性能

```json
input {
    generator {
        count => 2000000
        message => '{"key1":"value1","key2":[1,2],"key3":{"subkey1":"subvalue1"}}'
        codec => json
    }
}
 
output {
    stdout {
        codec => dots
    }
}
```

>/app/logstash/bin/logstash -f generator_dots.conf | pv -abt > /dev/null
>
>1.91MiB 0:00:55 [35.4KiB/s]



2】测试logstash输出性能

```json
input {
    generator {
        count => 2000000
        message => '{"key1":"value1","key2":[1,2],"key3":{"subkey1":"subvalue1"}}'
        codec => json
    }
}
 
output {
 
    stdout {
        codec => dots
    }
 
    elasticsearch {
      hosts => ["10.116.39.238:9200","10.116.39.239:9200","10.116.39.240:9200","10.116.39.241:9200","10.110.39.238:9200","10.110.39.239:9200","10.110.39.240:9200"]
      index => "test-%{+YYYY.MM.dd}"
    }
}
```

>/app/logstash/bin/logstash -f elasticsearch_test.conf | pv -abt > /dev/null
>
>1.91MiB 0:02:26 [13.3KiB/s]





### 4 logstash.conf

```bash
/opt/logstash/config/logstash.conf
```

```
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
      index => "nginx-access-%{+YYYY.MM.dd}"
   }
  }
  if [fields][log_source]=="nginx-error"{
    elasticsearch {
      hosts => ["http://192.168.1.31:9200","http://192.168.1.32:9200","http://192.168.1.33:9200"]
      action => "index"
      index => "nginx-error-%{+YYYY.MM.dd}"
   }
  }
}
```



### 5 启动/关闭

```bash
# --config.test_and_exit选项会解析您的配置文件并报告任何错误
bin/logstash -f first-pipeline.conf --config.test_and_exit

# --config.reload.automatic选项启用自动配置重新加载
bin/logstash -f first-pipeline.conf --config.reload.automatic

# 启动
nohup /opt/logstash/bin/logstash -f /opt/logstash/config/logstash_test.conf >/dev/null 2>&1 &

# 停止
ps -ef|grep logstash|awk '{print $2}'|grep -v "grep"|xargs kill -9

# 同一OS启动多个实例，需指定--path.data
nohup /opt/logstash/bin/logstash -f /opt/logstash/conf/esg-eip-core-5051.conf --path.data=/opt/logstash/data/5051 >/dev/null 2>&1 &
```

