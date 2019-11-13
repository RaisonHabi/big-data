**COALESCE:**  
COALESCE函数返回第一个非NULL参数。如果所有参数都为NULL，则返回NULL  
示例：  
SELECT COALESCE(NULL, 1, 2, 'W3Schools.com')  
返回 1

**join多表连接：**  
FROM (((表1 JOIN 表2 ON 表1.字段号=表2.字段号) JOIN 表3 ON 表1.字段号=表3.字段号) JOIN 表4 ON Member.字段号=表4.字段号) JOIN 表X ON Member.字段号=表X.字段号 

**Where语句：**  
Where语句不能直接使用列别名
