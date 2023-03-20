## 背 景
Hive 的优化分为join相关的优化和join无关的优化，从项目实际来说, join 相关的优化占了 Hive 优化的大部分内容，而 join 相关的优化又分为 mapjoin 可以解决的 join 优化和mapj oin 无法解决的 join 优化 。  

另外一点，其实之所以需要优化，基本的本质原因是因为数据倾斜导致的，HiveQL的各种优化方法基本都和数据倾斜密切相关，说白了就是HiveQL在书写的时候因为表设计的不佳或者语句写的不佳，导致了数据倾斜，本来可以分布式并行做的事情，结果硬生生的做成了单节点处理，数据严重倾斜
&nbsp;
## join相关的优化
### 大表join小表优化
MAPJION会把小表全部读入内存中，在map阶段直接拿另外一个表的数据和内存中表数据做匹配，  
而普通的equality join则是类似于mapreduce模型中的file join，需要先分组，然后再reduce端进行连接；  
由于mapjoin是在map是进行了join操作，省去了reduce的运行，效率也会高很多。

在Hive0.11后，Hive默认启动该优化，也就是不在需要显示的使用MAPJOIN标记，其会在必要的时候触发该优化操作将普通JOIN转换成MapJoin，可以通过以下两个属性来设置该优化的触发时机  
hive.auto.convert.join默认值为true，  
自动开户MAPJOIN优化 hive.mapjoin.smalltable.filesize 默认值为2500000(25M),  
通过配置该属性来确定使用该优化的表的大小，如果表的大小小于此值就会被加载进内存中

&nbsp;
## references  
[Hive Map Join 原理](https://cloud.tencent.com/developer/article/1481780)  
[Hive优化实践（十四）](https://juejin.cn/post/7084020528062677023)
