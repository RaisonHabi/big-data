## get_json_object(string json_string, string path)
说明：  
第一个参数填写json对象变量，第二个参数**使用$表示json变量标识** ，  
然后**用. 或 [] 读取对象或数组**；如果输入的json字符串无效，那么返回NULL。  
每次只能返回一个数据项。

&nbsp;
## 示例
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
## reference
[Hive解析json（get_json_object）](https://blog.csdn.net/qq_34105362/article/details/80454697)
