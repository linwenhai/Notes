### 1 URL检测

```python
"""URL检测"""
import subprocess

def check(ip):
    code="{http_code}"
    url="http://{0}:8080/mwmonitor/".format(ip)
    command="curl -o /dev/null -s -w '{0} http_code:%{1}\n' {2}".format(url,code,url)
    subprocess.call(command,shell=True)

with open('mserver_dr_ip.txt','r') as f:
    list=f.readlines()
    for i in list:
        ip=i.strip()
        check(ip)
```



2 

```shell
"""KMS-msever start"""
import subprocess

def shutdown(ip):
    command1 = "ssh -o StrictHostKeyChecking=no {0} '/app/jetty/logs/*.sh start'".format(ip)
    subprocess.call(command1,shell=True)

with open('mserverip_dr.txt','r') as f:
    list=f.readlines()
    for i in list:
        ip=i.strip()
        shutdown(ip)
```





