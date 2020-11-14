## 实例
一个customer对应多条assign、call_record，合并assign为json、call_record为json
```
INSERT OVERWRITE TABLE `${target.table}` 
select customer_no,
        collection_bill_id,
        product_code,
        create_time,
        appeal_content,
        concat("{",concat_ws(",",sort_array(collect_set(assign_kv_pair))),"}") as assign_json, 
        concat("{",concat_ws(",",sort_array(collect_set(call_kv_pair))),"}") as call_json
from(
    SELECT a.customer_no,
        a.collection_bill_id,
        a.product_code,
        a.create_time,
        a.appeal_content,
        concat('[\"assignment_time\":',NVL(assignment_time,''),',\"collector_code\":',NVL(b.collector_code,''),']') as assign_kv_pair,
        concat('[\"start_time\":',NVL(start_time,''),',\"collector_code\":',NVL(c.collector_code,''),',\"content\":',NVL(content,''),']') as call_kv_pair
    FROM(
        select customer_no,
                collection_bill_id,
                product_code,
                create_time,
                complaint_date,
                appeal_content,
                date_sub(complaint_date,3)as complaint_date_pre3,
                date_sub(complaint_date,60)as complaint_date_pre60
        from table1
    )a
    left join(
        select distinct customer_no,
                collection_bill_id,
                assignment_time,
                collector_code,
                collector_name,
                collection_group_code,
                collection_group_name,
                collection_queue_type,
                product_type,
                partition_date
        from table2
    )b on(a.customer_no=b.customer_no and a.collection_bill_id=b.collection_bill_id and a.product_code=b.product_type
        and b.assignment_time between a.complaint_date_pre60 and a.complaint_date 
        and b.partition_date between a.complaint_date_pre60 and a.complaint_date)
    left join(
        select distinct debtor_no,
                application_id,
                product_code,
                collector_code,
                start_time,
                content,
                partition_date
        from table3
    )c on(a.customer_no=c.debtor_no and a.collection_bill_id=c.application_id and a.product_code=c.product_code
        and c.partition_date between a.complaint_date_pre3 and a.complaint_date
        and c.start_time between a.complaint_date_pre3 and a.complaint_date)
##按字段顺序group by    
)group by 1,2,3,4,5
;

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
