## map的一些操作函数
### key键查询
```
-- map_keys(colName)  结果是一个Array,如果希望提取，则使用[index],如map_keys(smap)[0]
-- hive和prest的index起点存在差异,hive从0开始，presto从1开始【我测试的环境是这样的】
select map_keys(smap) as smap_keys,map_keys(imap) as imap_keys
from temp_db.map_test;
```
### value值查询
```
-- map_values(colname)
select map_values(smap) as s_values,map_values(imap) as i_values
from temp_db.map_test;
```
### 键值对查询
```
-- size(colName),返回对应列有多少个key-value
select size(imap) as pair_cnt
from temp_db.map_test;
```
### map类型数据的加工
将map列拆分为key、value列
```
-- smap中只存在单个key-value的情况，所有lateral之后，数据有单列变成双列。但是行数没有变化
select id,skey,svalue
from temp_db.map_test
lateral view explode(smap) tb as skey,svalue;

-- imap中 存在多个键值对。这顿操作之后，行数会增加
select id,ikey,ivalue
from temp_db.map_test
lateral view explode(imap) tb as ikey,ivalue;
```

&nbsp;
## Array操作
## Json的操作
## reference
[hive解析、处理复杂类型Map、Array、Json](https://www.jianshu.com/p/ef74c63c50f2)
