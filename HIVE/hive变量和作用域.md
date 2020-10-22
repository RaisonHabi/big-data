## 1.hivevar与hiveconf的区别
<table border="1" width="200" cellspacing="1" cellpadding="1"><tbody><tr><td>命名空间</td><td>使用权限</td><td>详细描述</td></tr><tr><td>hivevar</td><td>rw</td><td>用户自定义变量</td></tr><tr><td>hiveconf</td><td>rw</td><td>hive相关配置属性</td></tr><tr><td>system</td><td>rw</td><td>java定义的配置属性</td></tr><tr><td>env</td><td>r</td><td>Shell环境属性</td></tr></tbody></table>

&nbsp;
## 2.对于hivevar与hiveconf的使用
### hiveconf变量取值必须要使用hiveconf作为前缀参数，具体格式如下:

      ${hiveconf:key}  

### hivevar取值可以不使用前缀hivevar，具体格式如下：
使用前缀:

       ${hivevar:key}

不使用前缀:

       ${key}

&nbsp;
## 3.实例
Hive的变量前面有一个命名空间，包括三个hiveconf，system，env，还有一个hivevar
```
hiveconf的命名空间指的是hive-site.xml下面的配置变量值。
system的命名空间是系统的变量，包括JVM的运行环境。
env的命名空间，是指环境变量，包括Shell环境下的变量信息，如HADOOP_HOME之类的
```
实例：
```
set mapreduce.job.split.metainfo.maxsize=-1;
set hive.exec.parallel=true;
set day='2018-08-30';
set window_day=50
 
select
        *
from
    table_name
where
    dt >= date_sub(${hiveconf:day}, ${hiveconf:window_day})
    and dt <= ${hiveconf:day}
```

&nbsp;
## reference
[hive中的hiveconf与hivevar区别以及其作用域](https://blog.csdn.net/Dax1n/article/details/80822755)    
[Hive 中的变量](https://www.cnblogs.com/Allen-rg/p/9676109.html)
