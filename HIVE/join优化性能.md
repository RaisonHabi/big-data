## 小、大表 join
在小表和大表进行join时，将小表放在前边，效率会高。  
hive会将小表进行缓存。
## mapjoin
使用mapjoin将小表放入内存，在map端和大表逐一匹配。从而省去reduce。

当一个大表和一个或多个小表做JOIN时，最好使用MAPJOIN，性能比普通的JOIN要快很多。   
另外，MAPJOIN 还能解决数据倾斜的问题。  
MAPJOIN的基本原理是：在小数据量情况下，SQL会将用户指定的小表全部加载到执行JOIN操作的程序的内存中，从而加快JOIN的执行速度。

## 使用
在Hive0.11前，必须使用MAPJOIN来标记显示地启动该优化操作，由于其需要将小表加载进内存所以要注意小表的大小
```
SELECT /*+ MAPJOIN(smalltable)*/  .key,value
FROM smalltable JOIN bigtable ON smalltable.key = bigtable.key
```

在Hive0.11后，Hive默认启动该优化，也就是不在需要显示的使用MAPJOIN标记，其会在必要的时候触发该优化操作将普通JOIN转换成MapJoin，可以通过以下两个属性来设置该优化的触发时机
```
hive.auto.convert.join
```
默认值为true，自动开户MAPJOIN优化
```
hive.mapjoin.smalltable.filesize
```
默认值为2500000(25M),通过配置该属性来确定使用该优化的表的大小，如果表的大小小于此值就会被加载进内存中

 
注意：使用默认启动该优化的方式如果出现默名奇妙的BUG(比如MAPJOIN并不起作用),就将以下两个属性置为fase手动使用MAPJOIN标记来启动该优化
```
hive.auto.convert.join=false(关闭自动MAPJOIN转换操作)
hive.ignore.mapjoin.hint=false(不忽略MAPJOIN标记)
```
## reference
[hive大小表join优化性能](https://blog.csdn.net/u012036736/article/details/84978689)
