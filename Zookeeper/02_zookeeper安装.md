





 zoo.cfg 参数：

- tickTime =2000：通信心跳数Zookeeper 使用的基本时间，服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime时间就会发送一个心跳，时间单位为毫秒它用于心跳机制，并且设置最小的 session 超时时间为两倍心跳时间。(session的最小超时时间是2*tickTime)；
- initLimit =10：主从初始通信时限，集群中的 Follower 跟随者服务器与 Leader 领导者服务器之间初始连接时能容忍的最多心跳数（tickTime的数量），用它来限定集群中的 ZK 服务器连接到 Leader 的时限；
- syncLimit =5：主从同步通信时限，集群中 Leader 与 Follower 之间的最大响应时间单位，假如响应超过syncLimit * tickTime，Leader 认为 Follwer 死掉，从服务器列表中删除 Follwer；
- dataDir：数据文件目录+数据持久化路径；
- clientPort =2181：客户端连接端口



