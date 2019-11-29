## explode
***hive wiki对于expolde的解释如下：***  
**explode() takes in an array (or a map) as an input and outputs the elements of the array (map) as separate rows.   
UDTFs can be used in the SELECT expression list and as a part of LATERAL VIEW.**   
示例：  
> SELECT explode(myCol) AS myNewCol FROM myTable;  
*The usage with Maps is similar:*    
SELECT explode(myMap) AS (myMapKey, myMapValue) FROM myMapTable;  

**总结起来一句话：explode就是将hive一行中复杂的array或者map结构拆分成多行。**  
## lateral view
***hive wiki 上的解释如下：***  
Lateral View Syntax  
lateralView: LATERAL VIEW udtf(expression) tableAlias AS columnAlias (‘,’ columnAlias)*   
fromClause: FROM baseTable (lateralView)*   
***Description***  
**Lateral view is used in conjunction with user-defined table generating functions such as *explode()*.   
As mentioned in Built-in Table-Generating Functions, a UDTF generates zero or more output rows for each input row.   
A lateral view first applies the UDTF to each row of base table and then joins resulting output rows to the input rows to form a virtual table having the supplied table alias.**
## 示例
详见参考文档  
Lateral View通常和UDTF一起出现，为了解决UDTF不允许在select字段的问题。   
Multiple Lateral View可以实现类似笛卡尔乘积。   
Outer关键字可以把不输出的UDTF的空结果，输出成NULL，防止丢失数据。  


## reference
[hive lateral view 与 explode详解](https://blog.csdn.net/bitcarmanlee/article/details/51926530)  
[Hive Lateral view介绍](https://blog.csdn.net/youziguo/article/details/6837368)  
[Lateral View使用指南](https://blog.csdn.net/sunnyyoona/article/details/62894761)
