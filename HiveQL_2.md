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
