**1、to_date：日期时间转日期函数**  
示例：  
select to_date('2015-04-02 13:34:12');  
输出：2015-04-02  

**2、from_unixtime：转化unix时间戳到当前时区的时间格式**  
示例：  
select from_unixtime(1323308943);   
输出：2011-12-08 09:49:03  
select from_unixtime(1323308943,’yyyyMMdd’);  
输出：20111208

**3、unix_timestamp：获取当前unix时间戳**  
示例：  
select unix_timestamp();  
输出：1430816254  
select unix_timestamp('2015-04-30 13:51:20');  
输出：1430373080  

**4、year：返回日期中的年（month/day/hour/minute/second相同）**  
select year('2015-04-02 11:32:12');  
输出：2015
 
**5、weekofyear：返回日期在当前周数**  
select weekofyear('2015-05-05 12:11:1');  
输出：19

**6、datediff：返回开始日期减去结束日期的天数**  
select datediff('2015-04-09','2015-04-01');  
输出：8

**7、date_sub：返回日期前n天的日期**  
select date_sub('2015-04-09',4);  
输出：2015-04-05

**8、date_add：返回日期后n天的日期**  
select date_add('2015-04-09',4);  
输出：2015-04-13

**9、from_unixtime+ unix_timestamp Hive中yyyymmdd和yyyy-mm-dd日期之间的切换**  
*思想：先转换成时间戳，再由时间戳转换为对应格式。*  
--20171205转成2017-12-05   
select from_unixtime(unix_timestamp('20171205','yyyymmdd'),'yyyy-mm-dd') from dual;  
--2017-12-05转成20171205  
select from_unixtime(unix_timestamp('2017-12-05','yyyy-mm-dd'),'yyyymmdd') from dual;  
***Dual表***  
*dual表的概念来自oracle，数据库建立时即与数据字典一起初始化，该表只有一个varchar2类型名为dummy的字段，表数据只有一行“X”，用来查询一些系统信息，如select sysdate from dual; select user from dual;select seq.nextval from dual等。
为了能在hive中测试一些时间、数学、聚合函数，可以仿照oracle创建dual表。*  
[如何在Hive构造Dual表](https://www.jianshu.com/p/9238d860ef66)  

**10：Hive中取最近30天数据**  
datediff(CURRENT_TIMESTAMP ,gmt_create)<=30 

**11、Hive中 两个日期相差多少小时**  
select (unix_timestamp('2018-05-25 12:43:55') - unix_timestamp('2018-05-25 11:03:55'))/3600  
输出：1 **(去尾法？)**  

**12、Hive中 两个日期相差多少分钟**  
select (unix_timestamp('2018-05-25 12:03:55') - unix_timestamp('2018-05-25 11:03:55'))/60  
输出：60

**13、hive 计算某一个日期属于星期几，如2018-05-20 是星期日**  
SELECT IF(pmod(datediff('2018-05-20', '1920-01-01') - 3, 7)='0', 7, pmod(datediff('2018-05-20', '1920-01-01') - 3, 7))   
输出：7

**14、hive返回上个月第一天和最后一天**  
--上个月第一天  
select trunc(add_months(CURRENT_TIMESTAMP,-1),'MM')  
select concat(substr(add_months(from_unixtime(unix_timestamp(),'yyyy-MM-dd'),-1),1,7),'-01');   
--上个月最后一天  
select date_sub(trunc(CURRENT_TIMESTAMP,'MM'),1);  
***hive trunc***[hive trunc的一些用法](https://www.cnblogs.com/wenBlog/p/10848208.html)


**reference**  
[Hive常用日期函数整理](https://blog.csdn.net/u013421629/article/details/80450047)
