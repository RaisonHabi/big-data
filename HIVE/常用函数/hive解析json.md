## 一、get_json_object(string json_string, string path)
说明：  
第一个参数填写json对象变量，第二个参数**使用$表示json变量标识** ，  
然后**用. 或 [] 读取对象或数组**；如果输入的json字符串无效，那么返回NULL。  
每次只能返回一个数据项。

### 示例
data 为 test表中的字段，数据结构如下：
```
data =
{
 "store":
        {
         "fruit":[{"weight":8,"type":"apple"}, {"weight":9,"type":"pear"}],  
         "bicycle":{"price":19.95,"color":"red"}
         }, 
 "email":"amy@only_for_json_udf_test.net", 
 "owner":"amy" 
}
```
1.get单层值
```
hive> select  get_json_object(data, '$.owner') from test;
结果：amy
```
2.get多层值.
```
hive> select  get_json_object(data, '$.store.bicycle.price') from test;
结果：19.95
```
3.get数组值[]
```
hive> select  get_json_object(data, '$.store.fruit[0]') from test;
结果：{"weight":8,"type":"apple"}
```

&nbsp;
## 二、json_tuple
语法：json_tuple(json_string, k1, k2 ...)

说明：**解析json的字符串json_string,可指定多个json数据中的key，返回对应的value。如果输入的json字符串无效，那么返回NULL。**

示例：
```
select  
b.name 
,b.age 
from tableName a lateral view 
json_tuple('{"name":"zhangsan","age":18}','name','age') b as name,age; 
```
***注意：上面的json_tuple函数中没有$.，如果在使用json_tuple函数时加上$.就会解析失败。***

&nbsp;
## References
[Hive解析json（get_json_object）](https://blog.csdn.net/qq_34105362/article/details/80454697)   
[一文学会Hive解析Json数组](https://bigdata.51cto.com/art/202104/660074.htm)
