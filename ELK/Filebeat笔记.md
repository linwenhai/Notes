## Filebeat笔记



```sh
tar xzvf filebeat-7.2.0-linux-x86_64.tar.gz
```



### 1 filebeat.yml



1】定义输入日志路径.

```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/*.log
```



2】要将输出直接发送到Elasticsearch

```yaml
output.elasticsearch:
  hosts: ["myEShost:9200"]
```