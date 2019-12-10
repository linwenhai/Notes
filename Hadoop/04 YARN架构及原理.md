## YARN架构及原理

#### 1 背景

MapReduce本身存在着一些问题：
	1】JobTracker单点故障问题；如果Hadoop集群的JobTracker挂掉，则整个分布式集群都不能使用了。
	2】JobTracker承受的访问压力大，影响系统的扩展性。
	3】不支持MapReduce之外的计算框架，比如Storm、Spark、Flink等。

YARN是Hadoop2.0版本新引入的资源管理系统，直接从MR1演化而来。

**核心思想**：将MP1中JobTracker的资源管理和作业调度两个功能分开，分别由ResourceManager和ApplicationMaster进程来实现。
	1】ResourceManager：负责整个集群的资源管理和调度。
	2】ApplicationMaster：负责应用程序相关的事务，比如任务调度、任务监控和容错等。

YARN的出现，使得多个计算框架可以运行在一个集群当中。
	1】每个应用程序对应一个ApplicationMaster。
	2】目前可以支持多种计算框架运行在YARN上面比如MapReduce、Storm、Spark、Flink等。



#### 2 YARN的基本架构

从YARN的架构图来看，它主要由ResourceManager、NodeManager、ApplicationMaster和Container等组件组成。



##### ResourceManager（RM）

​	ResourceManager控制整个集群并管理应用程序向基础计算资源的分配。ResourceManager 将各个资源部分（计算、内存、带宽等）精心安排给基础NodeManager（YARN 的每节点代理）。ResourceManager还与 ApplicationMaster 一起分配资源，与NodeManager 一起启动和监视它们的基础应用程序。

​	1】处理客户端请求；
​	2】启动或监控ApplicationMaster；
​	3】监控NodeManager；
​	4】资源的分配与调度



##### NodeManager（NM）

​	NodeManager管理一个YARN集群中的每个节点。NodeManager提供针对集群中每个节点的服务，从监督对一个容器的终生管理到监视资源和跟踪节点健康。

​	1】单个节点上的资源管理；
​	2】处理来自ResourceManager上的命令；
​	3】处理来自ApplicationMaster上的命令。



##### ApplicationMaster（AM）

​	ApplicationMaster管理一个在YARN内运行的应用程序的每个实例。ApplicationMaster 负责协调来自 ResourceManager 的资源，并通过 NodeManager 监视容器的执行和资源使用（CPU、内存等的资源分配）。

​	1】负责数据的切分；
​	2】为应用程序申请资源并分配给内部的任务；
​	3】任务的监控与容错。



##### Container

​	对任务运行环境进行抽象，封装CPU、内存等多维度的资源以及环境变量、启动命令等任务运行相关的信息。比如内存、CPU、磁盘、网络等，当AM向RM申请资源时，RM为AM返回的资源便是用Container表示的。YARN会为每个任务分配一个Container，且该任务只能使用该Container中描述的资源。



```
要使用一个YARN集群，首先需要来自包含一个应用程序的客户的请求。ResourceManager 协商一个容器的必要资源，启动一个ApplicationMaster 来表示已提交的应用程序。通过使用一个资源请求协议，ApplicationMaster协商每个节点上供应用程序使用的资源容器。执行应用程序时，ApplicationMaster 监视容器直到完成。当应用程序完成时，ApplicationMaster 从 ResourceManager 注销其容器，执行周期就完成了。
```







#### 3 YARN 的作业运行

![img](assets/20180415111440631)

 

##### 3.1 作业提交

1】 client调用job.waitForCompletion方法，向整个集群提交MapReduce作业(第1步)；
2】 新的作业ID(应用ID)由RM分配(第2步)；
3】作业的client核实作业的输出，计算输入的split。将作业的资源(包括Jar包, 配置文件, split信息)拷贝给HDFS(第3步)；
4】 通过调用RM的submitApplication()来提交作业(第4步)；



##### 3.2 作业初始化

1】RM收到submitApplication()的请求时, 就将该请求发给调度器(scheduler), 调度器分配container；然后RM在该container内启动ApplicationMaster, 由NM监控(第5a和5b步)；
2】MapReduce作业的应用管理器是一个主类为MRAppMaster的Java应用，其通过创造一些bookkeeping对象来监控作业的进度, 得到任务的进度和完成报告(第6步)；
3】通过HDFS得到由客户端计算好的输入split(第7步)；
4】为每个输入split创建一个map任务, 根据mapreduce.job.reduces创建reduce任务对象；



##### 3.3 任务分配  

1】如果作业很小，ApplicationMaster会选择在其自己的JVM中运行任务。
2】如果不是小作业, 那么ApplicationMaster向RM请求container来运行所有的map和reduce任务(第8步)。 
3】这些请求是通过心跳来传输的, 包括每个map任务的数据位置，比如存放输入split的主机名和机架(rack)。调度器利用这些信息来调度任务, 尽量将任务分配给存储数据的节点, 或者退而分配给和存放输入split的节点相同机架的节点。



##### 3.4 任务运行

1】当一个任务由RM的调度分配给一个container后, ApplicationMaster通过联系NM来启动container(第9a步和9b步)
2】任务由一个主类为YarnChild的Java应用执行. 在运行任务之前首先本地化任务需要的资源, 比如作业配置, JAR文件, 以及分布式缓存的所有文件(第10步)
3】运行map或reduce任务(第11步)
4】YarnChild运行在一个专用的JVM中, 但是YARN不支持JVM重用。



##### 3.5 进度和状态更新 

1】YARN中的任务将其进度和状态（包括counter）返回给ApplicationMaster，客户端每秒（通过mapreduce.client.progressmonitor.pollinterval设置）向应用管理器请求进度更新，展示给用户。



##### 3.6 作业完成

1】 除了向ApplicationMaster请求作业进度外，客户端每5分钟都会通过调用waitForCompletion()来检查作业是否完成。时间间隔可以通过mapreduce.client.completion. pollinterval来设置。作业完成之后, ApplicationMaster和container会清理工作状态, OutputCommiter的作业清理方法也会被调用。作业的信息会被作业历史服务器存储以备之后用户核查。









