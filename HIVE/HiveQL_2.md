## COALESCE: 
COALESCE函数返回第一个非NULL参数。如果所有参数都为NULL，则返回NULL  
示例：  
SELECT COALESCE(NULL, 1, 2, 'W3Schools.com')  
返回 1

## Where语句：  
Where语句不能直接使用列别名，可以使用from内的别名

## substr:
***substr(string A, int start),substring(string A, int start)***  
两者用法一样,两个参数  
返回值: string  ,返回字符串A从start位置到结尾的字符串  
 示例：  
 > select substr('abcde',3) from lxw_dual;  
   cde  
select substring('abcde',3) from lxw_dual;  
               cde  
select substr('abcde',-1) from lxw_dual; （和ORACLE相同，负数从最后一位开始截取）  
e  
select substr('abcde',1,2) 和select substr('abcde',0,2)结果一样  
ab，默认都是从第一位开始取.  
select substr('abcde',6) (结果是空值)   

***substr(string A, int start, int len),substring(string A, intstart, int len)***  
用法一样，三个参数  
返回值: string, 返回字符串A从start位置开始，长度为len的字符串  
示例：  
> select substr('abcde',3,2) fromlxw_dual;  
cd  
select substring('abcde',3,2) fromlxw_dual;  
cd  
select substring('abcde',-2,2) fromlxw_dual;  
de  

## where子句子查询
*Hive 2.1.1版本中，支持where子句中的子查询 in 和 not in，但是 HSQL常用的exist 子句在Hive中是不支持的。*  
**LEFT SEMI JOIN （左半连接）是 IN/EXISTS 子查询的一种更高效的实现。可以用 LEFT SEMI JOIN 重写子查询语句**  
示例：  
> select a.key,a.value from a where a.key in (select b.key from b)  
可以改写为  
**select a.key,a.value from a left semi join b on (a.key=b.key)**   

***特点：***  
> 1、left semi join 的限制是， JOIN 子句中右边的表只能在 ON 子句中设置过滤条件，在 WHERE 子句、SELECT 子句或其他地方过滤都不行。  
2、left semi join 是只传递表的 join key 给 map 阶段，因此left semi join 中最后 select 的结果只许出现左表。  
3、因为 left semi join 是 in(keySet) 的关系，遇到右表重复记录，左表会跳过，而 join 则会一直遍历。这就导致右表有重复值得情况下 left semi join 只产生一条，join 会产生多条，也会导致 left semi join 的性能更。   

[hive 的 left semi join 讲解](https://blog.csdn.net/happyrocking/article/details/79885071)  
[Hive中HSQL中left semi join和INNER JOIN、LEFT JOIN、RIGHT JOIN、FULL JOIN区别](https://blog.csdn.net/helloxiaozhe/article/details/87910386)  
