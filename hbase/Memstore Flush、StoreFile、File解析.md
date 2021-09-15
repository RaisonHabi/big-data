## Memstore
Memstore是HBase框架中非常重要的组成部分之一，是HBase能够实现高性能随机读写至关重要的一环。深入理解Memstore的工作原理、运行机制以及相关配置，对hbase集群管理、性能调优都有着非常重要的帮助

HBase中，Region是集群节点上最小的数据服务单元，用户数据表由一个或多个Region组成。在Region中每个ColumnFamily的数据组成一个Store。每个Store由一个Memstore和多个HFile组成，如下图所示：

### Memstore 总结


&nbsp;
## StoreFile
1） 一个Region由多个Store组成，一个Store对应一个ColumnFamily（列族）
Store包括位于内存中的MemStore和位于磁盘的StoreFile;写操作先写入MemStore，当Memstore中的数据达到某个阈值，HRegionserver会启动flashcache进程写入StoreFile，每次写入形成单独的一个StoreFile

2）当StoreFile文件的数量增长到一定阈值后，系统会进行合并（minor、major compaction），在合并过程中会进行版本合并和删除工作（majar），形成更大的StoreFile

3）当一个Region所有StoreFile的大小和数量超过一定阈值后，会把当前的Region分割为两个，并由HMaster分配到相应的HRegionserver服务器，实现负载均衡

4）客户端检索数据，先在MemStore找，找不到再找StoreFile
### StoreFile 总结
当一个Store中的StoreFile达到一定的阈值后，就会
进行一次合并（major compact），将对同一个key的修改合并到一起
形成一个大的StoreFile，当StoreFile大小达到一定的阈值后，又会对
StoreFile进行Split，等分成两个StoreFile

&nbsp;
## References
[【Hbase】Memstore Flush、StoreFile、File解析](https://blog.csdn.net/qq_43733123/article/details/103552467)
