Flink 的一个重要特性就是有状态计算(stateful processing)。Flink 提供了简单易用的 API 来存储和获取状态。但是，我们还是要理解 API 背后的原理，才能更好的使用。
## 一. State 存储方式
Flink 为 state 提供了三种开箱即用的后端存储方式(state backend)：
```
Memory State Backend
File System (FS) State Backend
RocksDB State Backend
```

&nbsp;
## 二. Keyed State & Operator State
### 如果我们查看 flink官方文档，可以发现 flink 将 state 分成了两大类：    
Operator State (或者non-keyed state )   
每个 Operator state 绑定一个并行 Operator 实例。Kafka Connector 是使用 Operator state 的典型示例：每个并行的 kafka consumer 实例维护了每个 kafka topic 分区和该分区 offset 的映射关系，并将这个映射关系保存为 Operator state。
在算子并行度改变时，Operator State 也会重新分配。

Keyed State  
这种 State 只存在于 KeyedStream 上的函数和操作中，比如 Keyed UDF(KeyedProcessFunction…) window state 。可以把 Keyed State 想象成被分区的 Operator State。每个 Keyed State 在逻辑上可以看成与一个 <parallel-Operator-instance, key> 绑定，由于一个 key 肯定只存在于一个 Operator 实例，所以我们可以简单的认为一个 <operaor, key> 对应一个 Keyed State。
每个 Keyed State 在逻辑上还会被分配到一个 Key Group。分配方法如下：


### 根据 state 数据是否被 flink 托管，flink 又将 state 分类为 managed state 和 raw state：
```
managed state: 被 flink 托管，保存为内部的哈希表或者 RocksDB; checkpoint 时，flink 将 state 进行序列化编码。例如 ValueState ListState…

raw state: Operator 自行管理的数据结构，checkpoint 时，它们只能以 byte 数组写入 checkpoint。
```
当然建议使用 managed state 啦！使用 managed state 时， flink 会帮我们在更改并行度时重新分发 state，并且优化内存。

&nbsp;
## References
[Working with State](https://ci.apache.org/projects/flink/flink-docs-release-1.9/dev/stream/state/state.html)  
[Flink 如何保存状态数据](https://blog.csdn.net/yuchuanchen/article/details/102941569)   
[Flink 状态管理详解（State TTL、Operator state、Keyed state）](https://cloud.tencent.com/developer/article/1792720)
