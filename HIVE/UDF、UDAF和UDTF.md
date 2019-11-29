## 背景
Hive是基于Hadoop中的MapReduce，提供HQL查询的数据仓库。  
Hive是一个很开放的系统，很多内容都支持用户定制，包括：  
> a）文件格式：Text File，Sequence File  
b）内存中的数据格式： Java Integer/String, Hadoop IntWritable/Text  
c）用户提供的 map/reduce 脚本：不管什么语言，利用 stdin/stdout 传输数据  
d）用户自定义函数: Substr, Trim, 1 – 1  
e）用户自定义聚合函数: Sum, Average…… n – 1  
## 定义
**UDF(User-Defined Funcation)：**     
自定义函数，特点是输入一行，输出一行  
**UDAF(User-Defined Aggregation Funcation)：**   
自定义聚合函数，特点是输入多行，输出一行  
等同与SQL中常用的SUM()，AVG()，也是聚合函数    
**UDTF(User-Defined Table-Generating Functions):**  
自定义拆分函数，特点是输入一行，输出多行  
## reference
[hive中UDF、UDAF和UDTF使用](https://blog.csdn.net/liuj2511981/article/details/8523084)  

