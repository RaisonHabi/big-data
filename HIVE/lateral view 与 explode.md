## explode
***hive wiki对于expolde的解释如下：***  
**explode() takes in an array (or a map) as an input and outputs the elements of the array (map) as separate rows.   
UDTFs can be used in the SELECT expression list and as a part of LATERAL VIEW.**   
示例：  
> SELECT explode(myCol) AS myNewCol FROM myTable;  
*The usage with Maps is similar:*    
SELECT explode(myMap) AS (myMapKey, myMapValue) FROM myMapTable;  

**总结起来一句话：explode就是将hive一行中复杂的array或者map结构拆分成多行。**  
## reference
[hive lateral view 与 explode详解](https://blog.csdn.net/bitcarmanlee/article/details/51926530)  
[Hive Lateral view介绍](https://blog.csdn.net/youziguo/article/details/6837368)  
[Lateral View使用指南](https://blog.csdn.net/sunnyyoona/article/details/62894761)
