## Map/Reduce
Map/Reduce，是一种模式，适合解决并行计算的问题，比如TopN、贝叶斯分类等。注意，是并行计算，而非迭代计算，像涉及到层次聚类的问题就不太适合了。

从名字可以看出，这种模式有两个步骤，Map和Reduce。Map即数据的映射，用于把一组键值对映射成另一组新的键值对，而Reduce这个东东，以Map阶段的输出结果作为输入，对数据做化简、合并等操作。

而MapReduce是Hadoop生态系统中基于底层HDFS的一个计算框架，它的上层又可以是Hive、Pig等数据仓库框架，也可以是Mahout这样的数据挖掘工具。由于MapReduce依赖于HDFS，其运算过程中的数据等会保存到HDFS上，把对数据集的计算分发给各个节点，并将结果进行汇总，再加上各种状态汇报、心跳汇报等，其只适合做离线计算。和实时计算框架Storm、Spark等相比，速度上没有优势。旧的Hadoop生态几乎是以MapReduce为核心的，但是慢慢的发展，其扩展性差、资源利用率低、可靠性等问题都越来越让人觉得不爽，于是才产生了Yarn这个新的东东，并且二代版的Hadoop生态都是以Yarn为核心。Storm、Spark等都可以基于Yarn使用。

&nbsp;
## shuffle
Map 是映射， 负责数据的过滤分发； Reduce 是规约， 负责数据的计算归并。

Reduce 的数据来源于 Map， Map 的输出即是 Reduce 的输入， Reduce 需要通过 Shuffle 来获取数据。从 Map 输出到 Reduce 输入的整个过程可以广义地称为 Shuffle。

Shuffle 横跨 Map 端和 Reduce 端， 在 Map 端包括 Spill 过程， 在 Reduce 端包括 copy 和 sort 过程......

&nbsp;
## reference
[大数据系列之MapReduce的shuffle原理](https://zhuanlan.zhihu.com/p/134350819)  
[Mapreduce执行过程分析(基于Hadoop2.4)——(一)](https://www.cnblogs.com/scott007/p/3836687.html#top)  
[]()
