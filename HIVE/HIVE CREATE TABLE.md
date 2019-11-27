## CREATE TABLE 
## Create Table As Select (CTAS)
示例：
> CREATE TABLE new_key_value_store  
   ROW FORMAT SERDE "org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe"  
   STORED AS RCFile  
   AS  
SELECT (key % 1024) new_key, concat(key, value) key_value_pair  
FROM key_value_store  
SORT BY new_key, key_value_pair;  

新建的目标表可以使用一些表的属性，比如说serde和storage format。  
**并且若是select的字段进行了操作后没有取别名，表中的名字会被定义为_col0, _col1,_col2这种形式，**  
若是没有指定serde和storage format则会使用select 中表的serde和storage format   
使用这种建表语句，目标表必须服从这些条件，因此使用这种方式需要根据需求：   
> 1.目标表不能是分区表   
2.目标表不能是外部表   
3.The target table cannot be a list bucketing table.目标表不能使用CLUSTERED BY子句
## Create Table Like
示例：  
> CREATE TABLE empty_key_value_store  
LIKE key_value_store;

创建一个LIKE表的复制，但是不含有数据。

## reference
[HIVE CREATE TABLE(一)](https://blog.csdn.net/mhtian2015/article/details/78770176)   
[LanguageManual DDL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)
