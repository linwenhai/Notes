

### 一、JDK安装配置

下载：https://www.oracle.com/java/technologies/javase-downloads.html

OS：centos7.8

Java：[jdk-8u271-linux-x64.tar.gz](https://download.oracle.com/otn/java/jdk/8u271-b09/61ae65e088624f5aaa0b1d2d801acb16/jdk-8u271-linux-x64.tar.gz?AuthParam=1603683518_469f5ac2d460afd339370a06662200d4)

```bash
# jdk-8u271-linux-x64.tar.gz上传到/opt目录
cd /opt
tar -zxvf jdk-8u271-linux-x64.tar.gz
ln -s jdk-8u271-linux-x64 jdk
```



```bash
# 配置JDK环境变量
vi /etc/profile
```

>export JAVA_HOME=/opt/jdk
>export PATH=.:$JAVA_HOME/bin:$PATH
>export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

```bash
source /etc/profile
 
# 测试
java -version
```



### 二、Memory Dump

描述：生成堆转储快照dump文件。

```bash
jmap -dump:live,format=b,file=heapdump.phrof <pid>
```

分析工具：Memory Analyzer (MAT)

http://www.eclipse.org/mat/



### 三、Thread Dump

```bash
jstack <PID> > jstack.log
```

线程状态：

- RUNNABLE : 线程处于执行中
- BLOCKED : 线程被阻塞
- WAITING : 线程正在等待



分析工具：IBM Thread and Monitor Dump Analyzer for Java (TMDA)

https://www.ibm.com/support/pages/ibm-thread-and-monitor-dump-analyzer-java-tmda

```bash
java -jar jca*.jar
```



### 四、JVM参数

| 参数                     | 示例                       | 描述                                             |
| ------------------------ | -------------------------- | ------------------------------------------------ |
| -Xms                     | -Xms2G                     | JVM初始分配的堆内存                              |
| -Xmx                     | -Xmx8G                     | JVM最大允许分配的堆内存                          |
| -Xss                     | -Xss512k                   | 每个线程的堆栈大小，一般默认512~1024kb           |
| -Xmn                     | -Xmn256m                   | 年轻代大小，整个JVM内存=年轻代 + 年老代 + 持久代 |
| -XX:MetaspaceSize        | -XX:MetaspaceSize=512m     | 元空间大小                                       |
| -XX:MaxMetaspaceSize     | -XX:MaxMetaspaceSize=1024m | 用于设置metaspace区域的最大值                    |
| -XX:+PrintGCDetails      |                            | 打印GC详细日志信息                               |
| -XX:SurvivorRatio        |                            | 幸存者比例设置                                   |
| -XX:NewRatio             |                            | 新生代比例设置                                   |
| -XX:MaxTenuringThreshold |                            | 进入老年代阈值设置                               |