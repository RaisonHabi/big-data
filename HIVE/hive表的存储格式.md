## TEXTFILE  
默认格式，建表时不指定默认为这个格式，导入数据时会直接把数据文件拷贝到hdfs上不进行处理。  
源文件可以直接通过hadoop fs -cat 查看
## SEQUENCEFILE 
一种Hadoop API提供的二进制文件，使用方便、可分割、可压缩等特点。   
SEQUENCEFILE将数据以<key,value>的形式序列化到文件中。   
key为空，用value 存放实际的值， 这样可以避免map 阶段的排序过程。  
序列化和反序列化使用Hadoop 的标准的 Writable 接口实现。  
三种压缩选择：NONE, RECORD, BLOCK。 Record压缩率低，一般建议使用BLOCK压缩。  
> 使用时设置参数:  
        SET hive.exec.compress.output=true;  
        SET io.seqfile.compression.type=BLOCK; -- NONE/RECORD/BLOCK  
        create table test2(str STRING)  STORED AS SEQUENCEFILE; 
## RCFILE  
一种行列存储相结合的存储方式。  
首先，其将数据按行分块，保证同一个record在一个块上，避免读一个记录需要读取多个block。  
其次，块数据列式存储，有利于数据压缩和快速的列存取。  
理论上具有高查询效率（但hive官方说效果不明显，只有存储上能省10%的空间，所以不好用，可以不用）。  
**RCFile结合行存储查询的快速和列存储节省空间的特点**  
> 1）同一行的数据位于同一节点，因此元组重构的开销很低；  
> 2) 块内列存储，可以进行列维度的数据压缩，跳过不必要的列读取。  
  
查询过程中，在IO上跳过不关心的列。  
实际过程是，在map阶段从远端拷贝仍然拷贝整个数据块到本地目录，也并不是真正直接跳过列，而是通过扫描每一个row group的头部定义来实现的。    
但是在整个HDFS Block 级别的头部并没有定义每个列从哪个row group起始到哪个row group结束。  
所以在读取所有列的情况下，RCFile的性能反而没有SequenceFile高。
## ORC 
hive给出的新格式，属于RCFILE的升级版。  
而且数据可以压缩存储，压缩比和Lzo压缩差不多，比text文件压缩比可以达到70%的空间。    
而且读性能非常高，可以实现高效查询。  
具体介绍[LanguageManual ORC](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC)

## 自定义格式 
用户的数据文件格式不能被当前 Hive 所识别的，通过实现inputformat和outputformat来自定义输入输出格式
## 注意
只有TEXTFILE表能直接加载数据，本地load数据和external外部表直接加载运路径数据，都只能用TEXTFILE表。    
更深一步，hive默认支持的压缩文件（hadoop默认支持的压缩格式），也只能用TEXTFILE表直接读取,其他格式不行。  
可以通过TEXTFILE表加载后insert到其他表中。    
换句话说，SequenceFile、RCFile表不能直接加载数据，数据要先导入到textfile表，再从textfile表通过insert select from 导入到SequenceFile,RCFile表。   
SequenceFile、RCFile表的源文件不能直接查看，在hive中用select看。  
RCFile源文件可以用 hive --service rcfilecat /xxxxxxxxxxxxxxxxxxxxxxxxxxx/000000_0查看，但是格式不同，很乱。  
hive默认支持压缩文件格式参考[hive 压缩全解读](http://blog.csdn.net/longshenlmj/article/details/50550580)
## reference 
[hive表的存储格式; ORC格式的使用](https://blog.csdn.net/longshenlmj/article/details/51702343)
