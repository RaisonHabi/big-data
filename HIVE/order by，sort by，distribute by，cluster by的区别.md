## order by
order by会对输入做全局排序，因此只有一个Reducer(多个Reducer无法保证全局有序)。  
当输入规模较大时，消耗较长的计算时间。  

***注：如果在strict模式下使用order by语句，那么必须要在语句中加上limit关键字，因为执行order by的时候只能启动单个reduce，如果排序的结果集过大，那么执行时间会非常漫长。***

## sort by
sort by不是全局排序，其在数据进入reducer前完成排序，  
因此，如果用sort by进行排序，并且设置mapred.reduce.tasks>1，  
**则sort by只会保证每个reducer的输出有序，并不保证全局有序。**  

sort by不同于order by，它不受hive.mapred.mode属性的影响，sort by的数据只能保证在同一个reduce中的数据可以按指定字段排序。  
使用sort by你可以指定执行的reduce个数(通过set mapred.reduce.tasks=n来指定)，对输出的数据再执行归并排序，即可得到全部结果。

