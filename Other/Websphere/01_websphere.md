## Websphere

### 1 查询profile

```shell
./manageprofiles.sh -listProfiles
```

### 2 删除profile

```shell
./manageprofiles.sh -delete -profileName AppSrv01 rm -rf AppSrv01
```

### 3 创建profile

```shell
# 创建DM
/opt/ibm/bin/manageprofiles.sh -create -templatePath /opt/ibm/profileTemplates/dmgr -profilename Dmgr01 -profilepath /opt/ibm/profiles/Dmgr01
```

```shell
# 创建APP01
/opt/ibm/bin/manageprofiles.sh -create -templatePath /opt/ibm/profileTemplates/default -profilename AppSrv01 -profilepath /opt/ibm/profiles/AppSrv01 
```

```shell
# 主从节点联合*
/opt/ibm/profiles/Dmgr01/bin/startManager.sh	#启动DM 
./addNode.sh <Dmgr_ip地址> <Dmgr_SOAP端口> 			#节点联合 
# Dmgr_SOAP端口默认为8879，可以查看Dmgr/logs/AboutThisProfile.txt
```

### 4 was安装乱码

4.1 在/usr/share/fonts目录下如果没有zh_CN/TrueTye目录，就新建该目录，看看里面有没有zysong.ttf字库。没有就拷贝一个；

4.2 在JAVA_HOME下找到jre/lib/fonts目录在其下新建fallback文件夹，拷贝zysong。ttf到fallback下

之后设置LANG=zh_CN.gb18030LANGUAGE=zh_CN.18030.

安装nc，则界面中文显示正常 

###  5 Websphere彻底卸载

```shell
rm -rf /opt/.ibm 
rm -rf /root/vpd.properties
```

