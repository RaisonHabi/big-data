## 一、内存不足解决方案
MapReduce作业运行时，任务可能会失败，报out of memory错误。这个时候可以采用以下几个过程调优
### 1、简单粗暴： 加大内存
哪个阶段报错就增加那个阶段的内存。以reduce阶段为例，map阶段的类似
```
mapreduce.reduce.memory.mb=5120   //设置reduce container的内存大小
mapreduce.reduce.java.opts=-Xms2000m -Xmx4600m; //设置reduce任务的JVM参数
```

### 2、设置copy阶段buffer占用的内存大小
有时候将内存设置大不管用，案例如下：   

原因 这是reduce从map取数据阶段报的错，reduce从map取数阶段使用的buffer可以占到reduce任务最大堆的70%的内存。报错之前copy还在运行，而reduce阶段其他过程占用了超过30%的内存，这个时候copy阶段继续取数，扩展buffer的时候，申请不到内存就报错了

解决方案 设置copy阶段buffer占用的内存大小，将mapreduce.reduce.shuffle.input.buffer.percent设置成0.5甚至更小

参数在wiki上的解释 The percentage of memory to be allocated from the maximum heap size to storing map outputs during the shuffle.

&nbsp;
## 二、报错
```
Error: java.lang.IllegalArgumentException: Column has wrong number of index entries found: 1 expected:
...
```
增大MR任务脚本中的reduce内存大小即可   
reducerMem

&nbsp;
## reference
[Reduce内存不足的解决方案](https://blog.csdn.net/jiewuyou/article/details/50592233)
