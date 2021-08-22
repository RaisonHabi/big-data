## HAVING 子句
在 SQL 中增加 HAVING 子句原因是，**WHERE 关键字无法与聚合函数一起使用**。

**HAVING 子句可以让我们筛选分组后的各组数据**。

&nbsp;
## SQL HAVING 语法
```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

&nbsp;
## SQL HAVING 实例
1、现在我们想要查找总访问量大于 200 的网站。
我们使用下面的 SQL 语句：

实例
```
SELECT Websites.name, Websites.url, SUM(access_log.count) AS nums FROM (access_log
INNER JOIN Websites
ON access_log.site_id=Websites.id)
GROUP BY Websites.name
HAVING SUM(access_log.count) > 200;
```

2、现在我们想要查找总访问量大于 200 的网站，并且 alexa 排名小于 200。
我们在 SQL 语句中增加一个普通的 WHERE 子句：

实例
```
SELECT Websites.name, SUM(access_log.count) AS nums FROM Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id
WHERE Websites.alexa < 200 
GROUP BY Websites.name
HAVING SUM(access_log.count) > 200;
```

&nbsp;
## References
[SQL HAVING 子句](https://www.runoob.com/sql/sql-having.html)



