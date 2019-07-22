## Hive的连接3种连接方式

### 1 CLI连接

进入到 bin 目录下，直接输入命令：

```shell
hive 	#或者
hive --service cli
```



### 2 HiveServer2/beeline

**1】修改hadoop集群的hdfs-site.xml配置文件**

加入一条配置信息，表示启用 webhdfs

```xml
<property>
	<name>dfs.webhdfs.enabled</name>
	<value>true</value>
</property>
```



**2】修改hadoop集群的core-site.xml配置文件**

加入两条配置信息：表示设置 hadoop 的代理用户

```xml
<property>
	<name>hadoop.proxyuser.hadoop.hosts</name>
	<value>*</value>
</property>
<property>
	<name>hadoop.proxyuser.hadoop.groups</name>
	<value>*</value>
</property>
```

配置解析：
hadoop.proxyuser.hadoop.hosts 配置成*的意义，表示任意节点使用hadoop集群的代理用户hadoop都能访问hdfs 集群，hadoop.proxyuser.hadoop.groups 表示代理用户的组所属。



**3】启动hiveserver2服务**

```shell
cd /app/hive/bin
nohup hiveserver2 &
```



**4】启动 beeline 客户端去连接**

```shell
cd /app/hive/bin
beeline -u jdbc:hive//hadoop3:10000 -n hive
```

-u : 指定元数据库的链接信息
-n : 指定用户名和密码



### 3 Web UI

下载对应版本的 src 包：apache-hive-2.3.2-src.tar.gz

```shell
tar -zxvf apache-hive-2.3.2-src.tar.gz
cd ${HIVE_SRC_HOME}/hwi/web
jar -cvf hive-hwi-2.3.2.war *
cp hive-hwi-2.3.2.war ${HIVE_HOME}/lib/
```



**1】修改配置文件 hive-site.xml**

```xml
<property>
	<name>hive.hwi.listen.host</name>
	<value>0.0.0.0</value>
	<description>监听的地址</description>
</property>
<property>
	<name>hive.hwi.listen.port</name>
	<value>9999</value>
	<description>监听的端口号</description>
</property>
<property>
	<name>hive.hwi.war.file</name>
	<value>lib/hive-hwi-2.3.2.war</value>
	<description>war包所在的地址</description>
</property>
```



**2】复制所需 jar 包**

```shell
cp ${JAVA_HOME}/lib/tools.jar ${HIVE_HOME}/lib
cp ${JAVA_HOME}/lib/commons-el-1.0.jar ${HIVE_HOME}/lib
cp ${JAVA_HOME}/lib/jasper-compiler-5.5.23.jar ${HIVE_HOME}/lib
cp ${JAVA_HOME}/lib/jasper-runtime-5.5.23.jar ${HIVE_HOME}/lib
```



**3】安装 ant**

下载apache-ant-1.9.4-bin.tar.gz

```shell
cd /app
tar -zxvf apache-ant-1.9.4-bin.tar.gz
vi /etc/profile
export ANT_HOME=app/apache-ant-1.9.4 
export PATH=$PATH:$ANT_HOME/bin
ant -version
```



**4】启动web UI**

```shell
cd /app/hive/bin
nohup ./hive --service hwi &
```



**5 】访问web UI**

http://hadoop02:9999/hwi















