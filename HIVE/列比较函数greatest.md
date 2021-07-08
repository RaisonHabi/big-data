## greatest
greatest(col_a, col_b, ..., col_n)比较n个column的大小，过滤掉null，但是当某个column中是string，而其他是int/double/float等时，返回null。
```
select greatest(-1, 0, 5, 8) 
# 8
```

&nbsp;
## References
[Hive 列比较函数greatest](https://www.jianshu.com/p/0a3ed303ef47)
