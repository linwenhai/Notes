## Hbase shell



```bash
 bin/hbase shell
 hbase(main):018:0> exit
```



```bash
# 查看所有表
hbase(main):001:0> list
```



```bash
# 创建表t1，列族为f1
hbase(main):002:0> create 't1',{NAME => 'f1'}
```



```bash
# 描述表信息
hbase(main):004:0> describe 't1'
```



```bash
hbase(main):005:0> create 'test','c1','c2'
hbase(main):005:0>
put 'test','r1','c1:1','11'
put 'test','r1','c1:2','12'
put 'test','r1','c1:3','13'
put 'test','r1','c2:1','21'
put 'test','r1','c2:2','22'
put 'test','r2','c1:1','31'
put 'test','r2','c1:2','32'
hbase(main):014:0> scan 'test'
hbase(main):015:0> get 'test','r1',{COLUMN => 'c2:2'}
hbase(main):016:0> disable 'test'
hbase(main):017:0> drop 'test'
```





