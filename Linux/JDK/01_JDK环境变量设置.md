

## JDK环境变量设置



```shell
tar -zxvf jdk-8u201-linux-x64.tar.gz
ln -s jdk-8u201-linux-x64 jdk
vi /etc/profile
```

>添加如下内容：
>
>export JAVA_HOME=/app/jdk
>export PATH=$JAVA_HOME/bin:$PATH 
>export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

```shell
source /etc/profile
```

