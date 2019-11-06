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


**1. ROW_NUMBER() OVER(*PARTITION BY COLUMN ORDER BY COLUMN*):**  
简单的说row_number()从1开始，为每一条分组记录返回一个数字

**2. like与rlike的区别：**  
like不是正则，而是通配符。这个通配符可以看一下SQL的标准，例如%代表任意多个字符。  
rlike是正则，正则的写法与java一样。'\'需要使用'\\',例如'\w'需要使用'\\w’  
A RLIKE B ，表示B是否在A里面。而A LIKE B,则表示B是否是A  

**3. union all:**  
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

**4. day,zday:**  
返回$now表示时刻的day部分，带z前缀的表示会前补0  
$now 假设为2018-07-03 21:14:36  
$now.day 的结果：3；  
$now.zday 的结果：03  

**5. 窗口函数与分析函数:**  
*Hive窗口函数*  
+ FIRST_VALUE：取分组内排序后，截止到当前行，第一个值 
+ LAST_VALUE： 取分组内排序后，截止到当前行，最后一个值 
+ LEAD(col,n,DEFAULT) ：用于统计窗口内往下第n行值。第一个参数为列名，第二个参数为往下第n行（可选，默认为1），第三个参数为默认值（当往下第n行为NULL时候，取默认值，如不指定，则为NULL） 
+ LAG(col,n,DEFAULT) ：与lead相反，用于统计窗口内往上第n行值。第一个参数为列名，第二个参数为往上第n行（可选，默认为1），第三个参数为默认值（当往上第n行为NULL时候，取默认值，如不指定，则为NULL）  
示例：LAG( shorten_url IGNORE NULLS ) OVER ( PARTITION BY hit_day, session_id ORDER BY click_id )  


*OVER从句*  
+ 使用标准的聚合函数COUNT、SUM、MIN、MAX、AVG 
+ 使用PARTITION BY语句，使用一个或者多个原始数据类型的列 
+ 使用PARTITION BY与ORDER BY语句，使用一个或者多个数据类型的分区或者排序列 
+ 使用窗口规范，窗口规范支持以下格式：   
  + (ROWS | RANGE) BETWEEN (UNBOUNDED | [num]) PRECEDING AND ([num] PRECEDING | CURRENT ROW | (UNBOUNDED | [num]) FOLLOWING)
  + (ROWS | RANGE) BETWEEN CURRENT ROW AND (CURRENT ROW | (UNBOUNDED | [num]) FOLLOWING)
  + (ROWS | RANGE) BETWEEN [num] FOLLOWING AND (UNBOUNDED | [num]) FOLLOWING

当ORDER BY后面缺少窗口从句条件，窗口规范默认是 RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW.  
当ORDER BY和窗口从句都缺失, 窗口规范默认是 ROW BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING.  

**6.hive join on where**  
*on从句一般只包含简单的join字段，比较大小等筛选条件放在where从句中*   
示例：  
select   
        A.id,A.name,A.grade,A.dept,B.id,B.name   
from  
      A left outer join B  
on  
      A.dept = B.id  
where  
      A.grade >=80  
 

**Hive中排序和聚集:**  
[Hive中排序和聚集](https://www.cnblogs.com/skyl/p/4736477.html)  
//五种子句是有严格顺序的：  
where → group by → having → order by → limit  
//where和having的区别:  
//where是先过滤再分组(对原始数据过滤),where限定聚合函数  
hive> select count(*),age from tea where id>18 group by age;  
//having是先分组再过滤(对每个组进行过滤,having后只能跟select中已有的列)  
hive> select age,count(*) c from tea group by age having c>2;  
//group by后面没有的列,select后面也绝不能有(聚合函数除外)  
hive> select ip,sum(load) as c from logs  group by ip sort by c desc limit 5;  
//distinct关键字返回唯一不同的值(返回age和id均不相同的记录)  
hive> select distinct age,id from tea;  
//hive只支持Union All,不支持Union  
//hive的Union All相对sql有所不同,要求列的数量相同,并且对应的列名也相同,但不要求类的类型相同(可能是存在隐式转换吧)  
select name,age from tea where id<80  
union all  
select name,age from stu where age>18;  
***Order By特性：***  
对数据进行全局排序，只有一个reducer task，效率低下;  
与mysql中 order by区别在于：在 strict 模式下，必须指定 limit，否则执行会报错;  
对于分区表，还必须显示指定分区字段查询.  
***Sort BY特性：***  
可以有多个Reduce Task（以DISTRIBUTE BY后字段的个数为准）。也可以手工指定：set mapred.reduce.tasks=4;  
每个Reduce Task 内部数据有序，但全局无序 .  
***Distribute by特性：***  
按照指定的字段对数据进行划分到不同的输出 reduce 文件中;  
distribute by相当于MR 中的paritioner，默认是基于hash 实现的;  
distribute by通常与Sort by连用.  
***Cluster By特性：***  
如果 Sort By 和 Distribute By 中所有的列相同，可以缩写为Cluster By以便同时指定两者所使用的列;  
注意被cluster by指定的列只能是降序，不能指定asc和desc。一般用于桶表.  


**Hive数据类型:**  

|数据类型| 字节数|最小值|最大值|示例|  
|-------|------|----|---|---:|
|tinyint|1字节|-128|127|45Y|  
|smallint|2字节|-32768|32767|100S|
|int|4字节|-2,147,483,648|2,147,483,647|36|
|bigint|8字节|-9,223,372,036,854,775,808|9,223,372,036,854,775,807|2000L|

默认情况下，整数常量被当做INT处理，除非整数常量超出了INT类型的取值范围或者在整数常量跟着Y、S、L等后缀，则常量将会作为TINYINT、SMALLINT和BIGINT处理。Hive中的浮点数常量默认被当做DOUBLE类型。

Hive-0.11.0和Hive-0.12.0固定了DECIMAL类型的精度并限制为38位数字，
从Hive-0.13.0开始可以指定DECIMAL的规模和精度，当使用DECIMAL类型创建表时可以使用DECIMAL(precision, scale)语法。
DECIMAL默认为DECIMAL(10,0)。 
DECIMAL类型比DOUBLE类型为浮点数提供了精确的数值和更广的范围，DECIMAL类型存储了数值的精确地表示，而DOUBLE类型存储了非常接近数值的近似值。
当DOUBLE类型的近似值精度不够时可以使用DECIMAL类型，比如金融应用，等于和不等于检查以及舍入操作，当数值超出了DOUBLE类型的范围（< -10^308 or > 10^308）或者非常接近于0（-10^-308 < ... < 10^-308）时，也可以使用DECIMAL类型。
