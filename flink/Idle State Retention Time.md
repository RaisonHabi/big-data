在上一篇文章中，介绍了 Flink State TTL 机制，这项机制对于应对通用的状态暴增特别有效。然而，这个特性也有其缺陷，例如不能保证一定可以及时清理掉失效的状态，以及目前仅支持 Processing Time 时间模式等等，另外对于旧版本的 Flink（1.6 之前），State TTL 功能也无法使用。

针对 Table API 和 SQL 模块的持续查询/聚合语句，Flink 还提供了另一项失效状态清理机制，这就是本文要提到的 Idle State Retention Time 选项，Flink 很早就提供了这个选项，该特性是借助 Query Configuration 配置项来定义的，但很多人并未启用，也不理解其中隐藏的暗坑。本文将对这一特性做说明，并给出一些使用建议。
## 问题引入
同样以官网文档的案例为起点，这是一个持续查询的 GROUP BY 语句，它没有时间窗口的定义，理论上会无限地计算下去：
```
SELECT sessionId, COUNT(*) FROM clicks GROUP BY sessionId;
```
这就带来了一个问题：随着时间的不断推进，内存中积累的状态会越来越多，因为数据流是无穷无尽、持续流入的，Flink 并不知道如何丢弃旧的数据。在这种情况下，如果放任不管，那么迟早有一天作业的状态数达到了存储系统的容量极限，从而造成作业的崩溃。

针对这个问题，Flink 提出了空闲状态保留时间（Idle State Retention Time）的概念。通过为每个状态设置 Timer，如果这个状态中途被访问过，则重新设置 Timer；否则（如果状态一直未被访问，长期处于 Idle 状态）则在 Timer 到期时做状态清理。这样，就可以确保每个状态都能得到及时的清理。

通过调用 StreamQueryConfig 的 withIdleStateRetentionTime 方法，可以为这个 QueryConfig 对象设置最小和最大的清理周期。这样，Flink 可以保证最早和最晚的状态清理时间。

需要注意的是，**旧版本 Flink 允许只指定一个参数，表示最早和最晚清理周期相同，但是这样可能会导致同一时间段有很多状态都到期，从而造成瞬间的处理压力。新版本的 Flink 要求两个参数之间的差距至少要达到 5 分钟，从而避免大量状态瞬间到期，对系统造成的冲击。**

&nbsp;
## References
[Flink SQL 状态越来越多？Idle State Retention Time 特性概览](https://cloud.tencent.com/developer/article/1452854)   
