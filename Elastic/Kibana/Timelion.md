## Timelion



### 1 .es()表达式

```
.es()表达式
	index : 指明查询的索引，例：log-*
	q：指明查询条件，使用的是Lucene语法
	metric：聚合条件y轴显示内容，默认是个数,使用方法：metric=sum:time(time字段的总和)，可有多个metric
	offset：x轴的偏移值，划重点！将不同时间的数据进行对比需要用到offset=-1h，获取1h前的数据进行对比
	timefield：指定时间轴采用的字段，默认@timestamp
```



### 2 其他函数

```
.label()函数：附加到任何表达式以添加自定义标签。
.title()函数：附加到表达式的末尾，以添加具有有意义名称的标题。
.color()函数：更改任何系列的颜色，并接受标准颜色名称、十六进制值或分组系列的颜色模式。
.legend()函数：来设置图例的位置和样式。
```



### 3 例子

多个.es()表达式使用逗号分隔。

```
.es(index='next-sfpay-core-jdk-app-ticket*',timefield='@timestamp',q='hhtPayQr').label('HHT二维码生成请求量')
```

