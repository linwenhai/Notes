## Hadoop源码编译安装包

Hadoop版本：hadoop-2.8.5-src.tar.gz
OS：CentOS Linux release 7.6
PS：OS需可以联网

 

### 1 Requirements


\* Unix System
\* JDK 1.7+
\* Maven 3.0 or later
\* Findbugs 1.3.9 (if running findbugs)
\* ProtocolBuffer 2.5.0
\* CMake 2.6 or newer (if compiling native code), must be 3.0 or newer on Mac
\* Zlib devel (if compiling native code)
\* openssl devel (if compiling native hadoop-pipes and to get the best HDFS encryption performance)
\* Linux FUSE (Filesystem in Userspace) version 2.6 or above (if compiling fuse_dfs)
\* Internet connection for first build (to fetch all Maven and Hadoop dependencies)
\* python (for releasedocs)



###  2 依赖包安装

```bash
yum install -y openssl openssl-devel svn ncurses-devel zlib-devel libtool
yum install -y snappy snappy-devel bzip2 bzip2-devel lzo lzo-devel lzop autoconf automake
yum install -y gcc gcc-c++ make cmake
```



### 3 JDK安装

```shell
tar -zxvf jdk-8u201-linux-x64.tar.gz
ln -s jdk-8u201-linux-x64 jdk
vi /etc/profile
```

```shell
# 添加JDK环境变量
export JAVA_HOME=/opt/jdk
export PATH=$JAVA_HOME/bin:$PATH
```

```shell
source /etc/profile
java -version
```



###  4 Maven安装

```shell
tar -zxvf apache-maven-3.3.9-bin.tar.gz
vi /etc/profile
```

```shell
# 添加maven环境变量
export MAVEN_HOME=/opt/apache-maven-3.3.9
export MAVEN_OPTS="-Xms256m -Xmx512m"
export PATH=$MAVEN_HOME/bin:$JAVA_HOME/bin:$PATH
```

```shell
source /etc/profile
mvn -version
```

```shell
vi setting.xml
```

```shell
# 添加阿里云源 
   <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
```



###  5 Findbugs安装

```shell
tar -zxvf findbugs-1.3.9.tar.gz
vi /etc/profile
```

```shell
# 添加findbugs环境变量
export FINDBUGS_HOME=/opt/findbugs-1.3.9
export PATH=$FINDBUGS_HOME/bin:$MAVEN_HOME/bin:$JAVA_HOME/bin:$PATH
```

```shell
source /etc/profile
findbugs -version
```

 

### 6 protobuf安装

```shell
tar -xzvf protobuf-2.5.0.tar.gz
./configure
make && make install
protoc --version
```



### 7 编译安装

```shell
tar -zxvf hadoop-2.8.5-src.tar.gz
mvn clean package -Pdist,native -DskipTests -Dtar
```

```shell
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 15:03 min
[INFO] Finished at: 2019-03-30T07:24:27+08:00
[INFO] Final Memory: 203M/494M
[INFO] ------------------------------------------------------------------------
```

需联网下载编译依赖包，耗时比较长，出现“[INFO] BUILD SUCCESS”为编译结束。
编译后的文件存放于 /opt/hadoop-2.8.5-src/hadoop-dist/target/hadoop-2.8.5.tar.gz