### JDK



### 1 JDK环境变量

```shell
export JAVA_HOME=/app/jdk
export PATH=.:$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```



### 2 JVM dump

描述：生成堆转储快照dump文件。

```shell
jmap -dump:live,format=b,file=heapdump.phrof <pid>
```

分析工具Memory Analyzer (MAT)

http://www.eclipse.org/mat/



### 3 Thread dump

```shell
jstack <PID> > jstack.log
```

线程状态：

- RUNNABLE : 线程处于执行中
- BLOCKED : 线程被阻塞
- WAITING : 线程正在等待