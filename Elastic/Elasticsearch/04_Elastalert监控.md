## Elastalert监控

https://elastalert.readthedocs.io/en/latest/index.html

### 1 安装依赖包

```shell
yum -y install wget openssl openssl-devel gcc gcc-c++
```

### 2 安装Python3

```shell
wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tgz
tar xf Python-3.6.9.tgz
cd Python-3.6.9
./configure --prefix=/usr/local/python --with-openssl
make && make install
```

```shell
mv /usr/bin/python /usr/bin/python_old
ln -s /usr/local/python/bin/python3 /usr/bin/python
ln -s /usr/local/python/bin/pip3 /usr/bin/pip
```

```shell
pip install --upgrade pip
sed -i '1s/python/python2.7/g' /usr/bin/yum
sed -i '1s/python/python2.7/g' /usr/libexec/urlgrabber-ext-down
```

```shell
python -V
pip -V
```

### 3 安装elastalert

```shell
git clone https://github.com/Yelp/elastalert.git
cd elastalert
pip install "elasticsearch<7,>6"    #es是7，所以这里选用的版本是这个
pip install -r requirements.txt
python setup.py install
```

```shell
#安装成功后可以看到四个命令
ll /usr/local/python/bin/elastalert*
/usr/local/python/bin/elastalert
/usr/local/python/bin/elastalert-create-index
/usr/local/python/bin/elastalert-rule-from-kibana
/usr/local/python/bin/elastalert-test-rule
```

```shell
ln -s /usr/local/python/bin/elastalert* /usr/bin
#创建索引,ElastAlert会把执行记录存放到这个索引中
#索引名叫elastalert_status
elastalert-create-index
```

### 4 config.yaml

```
rules_folder: rules	#规则目录位置
run_every:	#多久去查询一下根据定义的规则去es查询是否有符合规则的字段
  minutes: 1
buffer_time:	#当查询开始一直到结束，最大的缓存时间
  minutes: 15
es_host: 192.168.198.11	#es地址
es_port: 9200	        #es端口
es_username: elastic	#es账户
es_password: 111qqq		#es密码
writeback_index: elastalert_status	#es里的索引
writeback_alias: elastalert_alerts
alert_time_limit:	 #如果alert当时没有发出去重试多久之后放弃发送
  days: 2
```

### 5 rules/nginx.yaml

```
es_host: 192.168.198.11 #es的IP地址
es_port: 9200 #es的端口
es_username: elastic  #es的用户名
es_password: 111qqq #es的密码
name: nginx   #报警邮件的标题
type: frequency #类型：频率
index: nginx-access-* #监控的索引,多个使用逗号隔开
num_events: 1 #时间内触发的次数
timeframe:
 minutes: 1   #时间,和上边的参数关联,1分钟内有1次会报警
filter:
- query:
    query_string:
      query: "message:403"
alert_text: "Ref Log [http://192.168.198.11]" #会在报警内容中显示
smtp_host: smtp.163.com #smtp的地址
smtp_port: 25 #端口
smtp_auth_file: /opt/elastalert/rules/smtp_auth_file.yaml       #用户密码的文件
email_reply_to: 18127002705@163.com             #发送邮件的邮箱
from_addr: 18127002705@163.com
alert:
- "email" #报警类型
email:	#收件人地址
- "498733843@qq.com"    
- "789123456@qq.com"
```

### 6 rules/smtp_auth_file.yaml

```
user: "18127002705@163.com"
password: "******"
```

### 7 测试规则的正确性

```shell
elastalert-test-rule --config config.yaml rules/nginx.yaml
```

### 8 启动elastalert

```shell
cd /opt/elastalert
nohup python -m elastalert.elastalert --verbose --config config.yaml --rule rules/nginx.yaml &
```

