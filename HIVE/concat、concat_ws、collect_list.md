## concat
语法: concat(string A, string B…)  
返回值: string  
说明：返回输入字符串连接后的结果，支持任意个输入字符串  

## concat_ws
将列表中一个user可能会占用多行转换为每个user占一行的目标表格式，实际是“列转行”  
> **1.CONCAT_WS(separator, str1, str2,...)**  

> **2.concat_ws(string delimiter, array<string>)**  
If the date field is not string, then convert it to string:***concat_ws(',',collect_set(cast(date as string)))***

## group_concat
***Hive does not have group_concat function***  

GROUP_CONCAT()是MySQL数据库提供的一个函数，通常跟GROUP BY一起用  
> GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val])

## collect_list()
根据某个字段分组后，把分在一组的数据合并在一起，默认分隔符’,’ 
## collect_set()
在collect_list()的基础上去重   
另：set聚合无序，可以使用sort_array()函数进行排序   
> select *,concat_ws(",",sort_array(collect_set(b) over(distribute by a))) c from a;

