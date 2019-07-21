

Hive 具有 SQL 数据库的外表，但应用场景完全不同，Hive 只适合用来做海量离线数 据统计分析，也就是数据仓库。

### 1 Hive架构

![1563719673479](assets/1563719673479.png)

**用户接口**: shell/CLI, jdbc/odbc, webui Command Line Interface
**跨语言服务** ： thrift server提供了一种能力，让用户可以使用多种不同的语言来操纵hive
**底层的Driver**： 驱动器Driver，编译器Compiler，优化器Optimizer，执行器Executor
**元数据存储系统** ： RDBMS MySQL



### 2 Hive执行过程

HiveQL 通过命令行或者客户端提交，经过 Compiler 编译器，运用 MetaStore 中的元数 据进行类型检测和语法分析，生成一个逻辑方案(Logical Plan)，然后通过的优化处理，产生 一个 MapReduce 任务。

 

### 3 Hive的数据组织

Hive的存储结构包括数据库、表、视图、分区和表数据等。
数据库、表、分区等等都对应HDFS上的一个目录。
表数据对应 HDFS 对应目录下的文件。

#### 3.1 Hive中包含以下数据模型

**database**：在 HDFS 中表现为${hive.metastore.warehouse.dir}目录下一个文件夹
**table**：在 HDFS 中表现所属 database 目录下一个文件夹
**external table**：与 table 类似，不过其数据存放位置可以指定任意 HDFS 目录路径
**partition**：在 HDFS 中表现为 table 目录下的子目录
**bucket**：在 HDFS 中表现为同一个表目录或者分区目录下根据某个字段的值进行 hash 散 列之后的多个文件
**view**：与传统数据库类似，只读，基于基本表创建

 

Hive中的表分为内部表、外部表、分区表和Bucket表

**内部表和外部表的区别**：
　　删除内部表，删除表元数据和数据
　　删除外部表，删除元数据，不删除数据

**分区表和分桶表的区别**： 
　　Hive数据表可以根据某些字段进行分区操作，细化数据管理，可以让部分查询更快。
        同时表和分区也可以进一步被划分为 Buckets，分桶表的原理和MapReduce编程中的HashPartitioner的原理类似。

　　分区和分桶都是细化数据管理，但是分区表是手动添加区分，由于Hive是读模式，所以对添加进分区的数据不做模式校验，分桶表中的数据是按照某些分桶字段进行hash散列形成的多个文件，所以数据的准确性也高很多。

