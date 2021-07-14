## percentile和percentile_approx
percentile：percentile(col, p) col是要计算的列（值必须为int类型），p的取值为0-1，若为0.2，那么就是2分位数，依次类推。

percentile_approx：percentile_approx(col, p)。列为数值类型都可以。

percentile_approx还有一种形式percentile_approx(col, p，B)，参数B控制内存消耗的近似精度，B越大，结果的精度越高。默认值为10000。当col字段中的distinct值的个数小于B时，结果就为准确的百分位数。

### 示例
```
select d, 
       percentile_approx(uv, array(0.2,0.4,0.6,0.8), 9999) as uv --2%分位数作为最小值       
  from aa
 group by d
```

&nbsp;
## References
[Hive SQL--如何使用分位数函数（percentile）](https://blog.csdn.net/Jarry_cm/article/details/82185576)
