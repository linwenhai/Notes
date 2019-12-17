keepalived与haproxy

#### 1 keepalived安装

**1】 源码安装**

```shell
tar -zxvf keepalived-1.3.4.tar.gz
cd keepalived-1.3.4
./configure --prefix=/app/keepalived
make && make install
cp /app/keepalived/sbin/keepalived /usr/sbin/
mkdir /etc/keepalived
cp /app/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf
cp /app/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
cd /app/keepalived-1.3.4
cp ./keepalived/etc/init.d/keepalived /etc/init.d/
chmod 755 /etc/init.d/keepalived
```

**2】yum安装**

```shell
yum install keepalived
```



**3】 keepalived.con配置**

```shell
vi /etc/keepalived/keepalived.conf
```

```
! Configuration File for keepalived

vrrp_script chk_http_haproxy {
        script "/etc/keepalived/check_haproxy.sh"
        interval 10
        weight 2
}

global_defs {
   notification_email {
        lin@shenlan.com							##故障发生时给谁发邮件通知
   }
   notification_email_from lin@shenlan.com		##通知邮件从哪个地址发出
   smtp_server mail.shenlan.com					##通知邮件的smtp地址
   smtp_connect_timeout 30
   router_id ceshi								##标识本节点的字条串，通常为hostname
}

vrrp_instance VI_1 {
    state MASTER				##备机设置 BACKUP
    interface eth0
    virtual_router_id 101		##主备一样
    priority 100				##备机设置90
    nopreempt
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass APP_NAME
    }

   track_script {
        chk_http_haproxy
}

    virtual_ipaddress {
        192.168.1.111/24
    }
}
```



**4】 check_haproxy.sh脚本配置**

```shell
vi /etc/keepalived/check_haproxy.sh
chmod 755 /etc/keepalived/check_haproxy.sh
```

```
#!/bin/bash
A=`ps -C haproxy --no-header |wc -l`
if [ $A -eq 0 ];then
/app/haproxy/sbin/haproxy -f /app/haproxy/conf/haproxy.cfg
sleep 3
if [ `ps -C haproxy --no-header |wc -l` -eq 0 ];then
/etc/init.d/keepalived stop
fi
fi
```



#### 2 haproxy的安装

```shell
wget http://fossies.org/linux/misc/haproxy-1.6.9.tar.gz
tar -zxvf haproxy-1.6.9.tar.gz
cd haproxy-1.6.9
make TARGET=linux2628 ARCH=x86_64 PREFIX=/usr/local/haproxy
make install PREFIX=/usr/local/haproxy
```

```
#参数说明
TARGET=linux26 #内核版本，使用uname -r查看内核，如：2.6.18-371.el5，此时该参数就为linux26；kernel 大于2.6.28的用：TARGET=linux2628
ARCH=x86_64 #系统位数
PREFIX=/usr/local/haprpxy #/usr/local/haprpxy为haprpxy安装路径
```



1】haproxy.cfg文件配置

```shell
vi /app/haproxy/haproxy.cfg
```

```
global
       log 127.0.0.1   local0 info
       maxconn 32768
       pidfile /app/haproxy/haproxy.pid
       uid 800
       gid 800
       daemon
       nbproc 1

defaults
       log     global
       mode    http
       option  httplog
       option  dontlognull
       retries 3
       option  redispatch
       option  forwardfor
       option  http-no-delay
       timeout connect 5000ms
       timeout client  5m
       timeout server  5m

       stats uri /haproxy_stats
       stats hide-version
       stats auth mwopr:tmall@748

listen http_esg_eims
       bind :80
       cookie SERVERID insert indirect
       hash-type consistent
       balance leastconn
       option httpchk HEAD /mwmonitor/check.jsp HTTP/1.0
       server s1 192.168.1.101:8080 cookie app1 check inter 2000 rise 2 fall 5
       server s2 192.168.1.102:8080 cookie app2 check inter 2000 rise 2 fall 5
```



#### 3 启动haproxy与keepalived

```shell
/opt/haproxy/sbin/haproxy -f /opt/haproxy/haproxy.cfg
/etc/init.d/keepalived start    #启动主节点A后的日志为:会广播ARP消息
tail -f /var/log/messages       #查看log消息
```



#### 4 测试

1] 两台机器上分别执行ip add，查看VIP是否在主上
2] 停掉主上的haproxy，3秒后keepalived会自动将其再次启动
3] 停掉主的keepalived，备机马上接管服务
4] 更改hosts
192.168.1.102 test.com
192.168.1.102 test.domain.com
通过IE测试，可以发现
test.com的请求发向了192.168.1.101:8080
test.domain.com的请求发向了192.168.1.101:8080

