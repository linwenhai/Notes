## MapReduce工作原理

#### 1 MapReduce 架构

​	在MapReduce中，用于执行MapReduce任务的机器角色有两个：JobTracker和TaskTracker。其中JobTracker是用于调度工作的，TaskTracker是用于执行工作的。一个Hadoop集群中只有一台JobTracker。
 



![img](https:////upload-images.jianshu.io/upload_images/6073827-f095ecb3e7847051.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/819/format/webp)



​	客户端向JobTracker提交一个作业，JobTracker把这个作业拆分成很多份，然后分配给TaskTracker去执行，TaskTracker会隔一段时间向JobTracker发送（Heartbeat）心跳信息，如果JobTracker在一段时间内没有收到TaskTracker的心跳信息，JobTracker会认为TaskTracker挂掉了，会把TaskTracker的作业任务分配给其他TaskTracker。



#### 2 MapReduce执行过程



![img](https:////upload-images.jianshu.io/upload_images/13946199-cbd524c9a15d8902.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/716/format/webp)

1】客户端启动一个job。
2】向JobTracker请求一个JobID。
3】将运行作业所需要的资源文件复制到HDFS上，包括MapReduce程序打包的JAR文件、配置文件和客户端计算所得的输入划分信息。这些文件都存放在JobTracker专门为该作业创建的文件夹中，文件夹名为该作业JobID。JAR文件默认会有10个副本，输入划分信息告诉JobTracker应该为这个作业启动多少个map任务等信息。
4】JobTracker接收到作业后将其放在作业队列中，等待JobTracker对其进行调度。当JobTracker根据自己的调度算法调度该作业时，会根据输入划分信息为每个划分创建一个map任务，并将map任务分配给TaskTracker执行。这里需要注意的是，map任务不是随便分配给某个TaskTracker的，Data-Local（数据本地化）将map任务分配给含有该map处理的数据库的TaskTracker上，同时将程序JAR包复制到该TaskTracker上运行，但是分配reducer任务时不考虑数据本地化。
5】TaskTracker每隔一段时间给JobTracker发送一个Heartbeat告诉JobTracker它仍然在运行，同时心跳还携带很多比如map任务完成的进度等信息。当JobTracker收到作业的最后一个任务完成信息时，便把作业设置成“成功”，JobClient再传达信息给用户。



#### 3 MapReduce中的Map和Reduce

​	MapReduce 框架只操作键值对，MapReduce 将job的不同类型输入当做键值对来处理并且生成一组键值对作为输出。

> 举个例子，我们要数图书馆中的所有书。你数1号书架，我数2号书架。这就是“Map”。我们人越多，数书就更快。现在我们到一起，把所有人的统计数加在一起。这就是“Reduce”。简单来说，Map就是“分”而Reduce就是“合” 。

(input) ->map->  ->combine->  ->reduce-> (output)



![img](https:////upload-images.jianshu.io/upload_images/13946199-2be81ee392b7f3ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)





![img](https:////upload-images.jianshu.io/upload_images/13946199-65c68075897015c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/725/format/webp)



#### 4 Map

​	MapReduce中的每个map任务可以细分4个阶段：record reader、mapper、combiner和partitioner。map任务的输出被称为中间键和中间值，会被发送到reducer做后续处理。
 



![img](https:////upload-images.jianshu.io/upload_images/13946199-c9c12883d7898d6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/528/format/webp)



1】读取HDFS中的文件，每一行解析成一个<k,v>，每一个键值对调用一次map函数，<0,helloyou>   <10,hello me>
2】覆盖map()，接收1中产生的<k,v>，进行处理，转换为新的<k,v>输出。<hello,1><you,1> <hello,1> <me,1>
3】对2输出的<k,v>进行分区。默认分为一个区。
4】对不同分区中的数据进行排序（按照k）、分组。分组指的是相同key的value放到一个集合中。排序后：<hello,1> <hello,1><me,1> <you,1>  分组后：<hello,{1,1}><me,{1}><you,{1}>
5】对分组后的数据进行归约（可选）。



#### 5 reduce

​	reduce任务可以分为4个阶段：混排（shuffle）、排序（sort）、reducer和输出格式（output format）
 



![img](https:////upload-images.jianshu.io/upload_images/13946199-8e5e6431d9da3c23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/698/format/webp)



1】多个map任务的输出，按照不同的分区，通过网络copy到不同的reduce节点上（shuffle）。
2】对多个map的输出进行合并、排序。覆盖reduce函数，接收的是分组后的数据，实现自己的业务逻辑，　<hello,2><me,1> <you,1>处理后，产生新的<k,v>输出。
3】对reduce输出的<k,v>写到HDFS中。





