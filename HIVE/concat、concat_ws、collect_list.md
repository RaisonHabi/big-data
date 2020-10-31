## 实例
```

```

&nbsp;
## concat
语法: concat(string A, string B…)  
返回值: string  
说明：返回输入字符串连接后的结果，支持任意个输入字符串  

&nbsp;
## concat_ws
将列表中一个user可能会占用多行转换为每个user占一行的目标表格式，实际是“列转行”  
> **1.CONCAT_WS(separator, str1, str2,...)**  

> **2.concat_ws(string delimiter, array<string>)**  
If the date field is not string, then convert it to string:***concat_ws(',',collect_set(cast(date as string)))***

&nbsp;
## collect_list()
根据某个字段分组后，把分在一组的数据合并在一起，默认分隔符’,’ 
## collect_set()
**在collect_list()的基础上去重**     
另：set聚合无序，可以使用sort_array()函数进行排序   
> select *,concat_ws(",",sort_array(collect_set(b) over(distribute by a))) c from a;

&nbsp;
## NVL函数
NVL(expr1, expr2)：
```
1、空值转换函数；
2、类似于mysql-nullif(expr1, expr2)，sqlserver-ifnull(expr1, expr2)。

备注：
1、如果expr1为NULL，返回值为 expr2，否则返回expr1。
2、适用于数字型、字符型和日期型，但是 expr1和expr2的数据类型必须为同类型。
```

&nbsp;
## group_concat
***Hive does not have group_concat function***  

GROUP_CONCAT()是MySQL数据库提供的一个函数，通常跟GROUP BY一起用  
> GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val])

&nbsp;
## reference
[Hive笔记之collect_list/collect_set（列转行）](https://www.cnblogs.com/cc11001100/p/9043946.html)   
[【Hive】NVL函数](https://blog.csdn.net/qq_34105362/article/details/80402806)   
[hive拼接两个字段组成json](https://blog.csdn.net/kaaosidao/article/details/89228925)   
