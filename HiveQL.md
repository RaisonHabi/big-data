**0. 从SQL到HiveQL应改变的几个习惯:**  
+ *IN:*  
SQL中可以使用IN操作符来规定多个值：  
SELECT * FROM Persons WHERE LastName IN ('Adams','Carter');  
HiveQL目前是不支持IN操作符的，需要通过转换为多个OR连接的条件：  
SELECT * FROM Persons WHERE LastName = 'Adams' OR LastName = 'Carter’;  
+ *INNER JOIN:*  
SQL中对两表内联可以写成：  
SELECT a.col, b.col FROM t1 a, t2 b WHERE a.id=b.id;  
**但这在HiveQL中是不支持的，需转为JOIN关键字的写法，如:**   
SELECT a.col, b.col FROM t1 a JOIN t2 b ON a.id=b.id;  


**1. ROW_NUMBER() OVER(*PARTITION BY COLUMN ORDER BY COLUMN*)**  
简单的说row_number()从1开始，为每一条分组记录返回一个数字

**2. like与rlike的区别：**  
like不是正则，而是通配符。这个通配符可以看一下SQL的标准，例如%代表任意多个字符。  
rlike是正则，正则的写法与java一样。'\'需要使用'\\',例如'\w'需要使用'\\w’  
A RLIKE B ，表示B是否在A里面。而A LIKE B,则表示B是否是A  

**3. union all**  
+ mysql中union用于合并两个或多个 SELECT 语句的结果集，LIMIT 用于对结果集进行分页显示。  
在 MySQL UNION 中使用 LIMIT 用于限制返回的记录条数，如果对 SELECT 子句做限制，需要对 SELECT 添加圆括号：  
(SELECT aid,title FROM article LIMIT 2)   
UNION ALL  
(SELECT bid,title FROM blog LIMIT 2)  
该 SQL 会返回个各SELECT 语句的两条记录，如果不添加圆括号，则最后一个 LIMIT 2 会作用于整个 UNION 语句而一共返回 2 条记录。  
+ 同 ORDER BY 类似，当需要对整个 UNION 的结果进行 LIMIT 限制时，建议将各个 SELECT 语句用圆括号括起来以使语句更加清晰：  
(SELECT aid,title FROM article)   
UNION ALL  
(SELECT bid,title FROM blog)  
LIMIT 2  
可见，LIMIT 与 ORDER BY 经常搭配使用，二者在 UNION 中的使用方式也是一致的。
