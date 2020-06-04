

```shell
wget ftp://sfftp:sf123456@10.110.39.241/filebeat-7.2.1.zip
unzip filebeat-7.2.1.zip
cd filebeat-7.2.1
chmod 755 filebeat
wget ftp://sfftp:sf123456@10.110.39.241/filebeat_standard.yml
sed -i "s#4#2#" filebeat_standard.yml
sed -i 's#test#esg-mrm-core-jetty-app-admin#' filebeat_standard.yml
nohup ./filebeat -e -c filebeat_standard.yml>/dev/null 2>&1 &
```







