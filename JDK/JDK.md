1 配置环境变量
export JAVA_HOME=/opt/jdk
export PATH=.:$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar



2 生成堆内存dump
jmap -dump:live,format=b,file=heap.bin <PID>　



3 生成线程栈输出
jstack PID > jstack.log

