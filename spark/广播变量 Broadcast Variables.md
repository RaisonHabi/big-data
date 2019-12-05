## Broadcast
**简介**
总的来说，每一个Spark的应用，都是由一个驱动程序（driver program）构成，它运行用户的main函数，在一个集群上执行各种各样的并行操作。  
Spark提出的最主要抽象概念是弹性分布式数据集 (resilient distributed dataset,RDD)，它是元素的集合，划分到集群的各个节点上，可以被并行操作。  
RDDs的创建可以从HDFS(或者任意其他支持Hadoop文件系统) 上的一个文件开始，或者通过转换驱动程序（driver program）中已存在的Scala集合而来。  
用户也可以让Spark保留一个RDD在内存中，使其能在并行操作中被有效的重复使用。  
最后，RDD能自动从节点故障中恢复。

Spark的第二个抽象概念是共享变量（shared variables），可以在并行操作中使用。  
在默认情况下，Spark通过不同节点上的一系列任务来运行一个函数，它将每一个函数中用到的变量的拷贝传递到每一个任务中。  
有时候，一个变量需要在任务之间，或任务与驱动程序之间被共享。  
Spark 支持两种类型的共享变量：  
> 广播变量 （broadcast variables），可以在内存的所有的结点上缓存变量；  
累加器 （accumulators）：只能用于做加法的变量，例如计数或求和。

**示例：**
> val broadcastVar = sc.broadcast(Array(1, 2, 3))  
broadcastVar: spark.Broadcast[Array[Int]] = spark.Broadcast(b5c40191-a864-4c7d-b9bf-d87e1a4e787c)  
 broadcastVar.value  
res0: Array[Int] = Array(1, 2, 3)  

[Spark 开发指南](https://colobu.com/2014/12/08/spark-programming-guide/)
