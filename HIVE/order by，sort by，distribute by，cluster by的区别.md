## order by
order by会对输入做全局排序，因此只有一个Reducer(***order by会默认设置reduce的个数=1***)。  
当输入规模较大时，消耗较长的计算时间。  

如果指定了hive.mapred.mode=strict（默认值是nonstrict）,这时就必须指定limit来限制输出条数  
***注：如果在strict模式下使用order by语句，那么必须要在语句中加上limit关键字，因为执行order by的时候只能启动单个reduce，如果排序的结果集过大，那么执行时间会非常漫长。***

## sort by
sort by不是全局排序，其在数据进入reducer前完成排序，  
因此，如果用sort by进行排序，并且设置mapred.reduce.tasks>1，  
**则sort by只会保证每个reducer的输出有序，并不保证全局有序。**  

sort by不同于order by，它不受hive.mapred.mode属性的影响，**sort by的数据只能保证在同一个reduce中的数据可以按指定字段排序。**     
使用sort by你可以指定执行的reduce个数(通过set mapred.reduce.tasks=n来指定)，对输出的数据再执行归并排序，即可得到全部结果。

## distribute by
distribute by是控制在map端如何拆分数据给reduce端的。  
hive会根据distribute by后面列，对应reduce的个数进行分发，默认是采用hash算法。  
sort by为每个reduce产生一个排序文件。  
***在有些情况下，你需要控制某个特定行应该到哪个reducer，这通常是为了进行后续的聚集操作。***    
**distribute by刚好可以做这件事。因此，distribute by经常和sort by配合使用。**  

注：Distribute by和sort by的使用场景  
> 1.Map输出的文件大小不均。  
2.Reduce输出文件大小不均。  
3.小文件过多。  
4.文件超大。

**示例：**   
> From table select year, temperature   
**distribute by year**    
***sort by year asc, temperature desc;***     
上面实现了局部排序，且规定了：根据年份和气温对气象数据进行排序，以**确保所有具有相同年份的行最终都在一个reducer分区中（文件下）**。    
可以看出，distribute by经常与sort by一起使用。

## cluster by
cluster by除了具有distribute by的功能外还兼具sort by的功能。  
**但是排序只能是倒叙排序，不能指定排序规则为ASC或者DESC。**





