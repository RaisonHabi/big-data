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
## reference
[官方文档](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/Drop/AlterView)   
[Hive中视图机制的初步使用及分析](https://www.cnblogs.com/panfeng412/archive/2013/04/29/hive-view-usage-and-analysis.html)  
