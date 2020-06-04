

```shell
# 查看物理CPU个数
cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l
```



```shell
# 查看逻辑CPU的个数
cat /proc/cpuinfo| grep "processor"| wc -l
```

