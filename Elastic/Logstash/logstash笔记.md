

1 basic Logstash pipeline

![1578560762472](assets/1578560762472.png)



```sh
cd logstash-7.2.1
bin/logstash -e 'input { stdin { } } output { stdout {} }'
```















