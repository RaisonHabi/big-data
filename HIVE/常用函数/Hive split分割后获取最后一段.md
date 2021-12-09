## 
可以先把这个字符串翻转过来(reverse)，split后直接去索引为0的，然后再翻转回去
```
select reverse(split(reverse(TXTMD),'\\\\')[0]) as size from xx_table
```

&nbsp;
## References
[Hive split分割后获取最后一段](https://www.cnblogs.com/EnzoDin/p/7341937.html)
