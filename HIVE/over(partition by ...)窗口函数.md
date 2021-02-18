## min() over(partition by ...)
OVER (PARTITION BY OtherField) is a window function. In addition, the main idea it to group by partitions without reducing of count of selected table rows.

In general, window functions are going to be faster than join/aggregation solutions. This is a rather simple case, so the performance might be essentially the same.

示例：  
SELECT MIN(Field) OVER (PARTITION BY OtherField) as Value FROM MYTABLE

&nbsp;
## reference
[select min over partition by](https://stackoverflow.com/questions/54803314/sql-server-query-select-min-over-partition-by)
