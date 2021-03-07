## View
Hive 0.6版本及以上支持视图（View，详见Hive的RELEASE_NOTES.txt），Hive View具有以下特点：
```
1）View是逻辑视图，暂不支持物化视图（后续将在1.0.3版本以后支持）；
2）View是只读的，不支持LOAD/INSERT/ALTER。需要改变View定义，可以是用Alter View；
3）View内可能包含ORDER BY/LIMIT语句，假如一个针对View的查询也包含这些语句， 则View中的语句优先级高；
4）支持迭代View。
```

### 语法
```
CREATE VIEW [IF NOT EXISTS] [db_name.]view_name [(column_name [COMMENT column_comment], ...) ]
  [COMMENT view_comment]
  [TBLPROPERTIES (property_name = property_value, ...)]
  AS SELECT ...;
```

&nbsp;
## 普通视图和物化视图
普通视图和物化视图的区别   
答曰：普通视图和物化视图根本就不是一个东西，说区别都是硬拼到一起的，首先明白基本概念，普通视图是不存储任何数据的，他只有定义，在查询中是转换为对应的定义SQL去查询，而物化视图是将数据转换为一个表，实际存储着数据，这样查询数据，就不用关联一大堆表，如果表很大的话，会在临时表空间内做大量的操作。

普通视图的三个特征：
1、**是简化设计，清晰编码的东西，他并不是提高性能的，他的存在只会降低性能**（如一个视图7个表关联，另一个视图8个表，程序员不知道，觉得很方便，把两个视图关联再做一个视图，那就惨了），他的存在未了在设计上的方便性  
2、其次，**是安全**，在授权给其他用户或者查看角度，多个表关联只允许查看，不允许修改，单表也可以同WITH READ ONLY来控制，当然有些项目基于视图做面向对象的开发，即在视图上去做INSTAND OF触发器，就我个人而言是不站同的，虽然开发上方便，但是未必是好事。  
3、**从不同的角度看不同的维度**，视图可以划分维度和权限，并使多个维度的综合，也就是你要什么就可以从不同的角度看，而表是一个实体的而已，一般维度较少（如：人员表和身份表关联，从人员表可以查看人员的维度统计，从身份看，可以看不同种类的身份有那些人或者多少人），其次另一个如系统视图USER_TABLE、TAB、USER_OBJECTS这些视图，不同的用户下看到的肯定是不一样的，看的是自己的东西。

物化视图呢，用于OLAP系统中，当然部分OLTP系统的小部分功能未了提高性能会借鉴一点点，因为表关联的开销很大，所以在开发中很多人就像把这个代价交给定期转存来完成，ORACLE当然也提供了这个功能，就是将视图（或者一个大SQL）的信息转换为物理数据存储，然后提供不同的策略：定时刷还是及时刷、增量刷还是全局刷等等可以根据实际情况进行选择，总之你差的是表，不是视图。



&nbsp;
## reference
[官方文档](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/Drop/AlterView)   
[Hive中视图机制的初步使用及分析](https://www.cnblogs.com/panfeng412/archive/2013/04/29/hive-view-usage-and-analysis.html)   
[普通视图和物化视图的区别](https://blog.csdn.net/joshua_peng1985/article/details/6213593)
