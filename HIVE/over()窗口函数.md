## sum(col) over()
```
##sum_spv相当于是总和
select cookieid,spv,sum(spv) over() as sum_spv
from(
select cookieid,sum(pv) as spv
from table1
group by cookieid
) t
```

&nbsp;
## min() over(partition by ...)
OVER (PARTITION BY OtherField) is a window function. In addition, the main idea it to group by partitions without reducing of count of selected table rows.

In general, window functions are going to be faster than join/aggregation solutions. This is a rather simple case, so the performance might be essentially the same.

示例：  
SELECT MIN(Field) OVER (PARTITION BY OtherField) as Value FROM MYTABLE

&nbsp;
## reference
[业务分析：hive下的分组求占比情况](https://blog.csdn.net/OYY_90/article/details/89843016)  
[select min over partition by](https://stackoverflow.com/questions/54803314/sql-server-query-select-min-over-partition-by)
