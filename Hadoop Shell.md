## FS Shell 
调用文件系统(FS)Shell命令应使用 bin/hadoop fs <args>的形式。   
所有的的FS shell命令使用URI路径作为参数。URI格式是scheme://authority/path。  
对HDFS文件系统，scheme是hdfs，对本地文件系统，scheme是file。其中scheme和authority参数都是可选的，如果未加指定，就会使用配置中指定的默认scheme。  
一个HDFS文件或目录比如/parent/child可以表示成hdfs://namenode:namenodeport/parent/child，或者更简单的/parent/child（假设你配置文件中的默认值是namenode:namenodeport）。  
大多数FS Shell命令的行为和对应的Unix Shell命令类似，不同之处会在下面介绍各命令使用详情时指出。出错信息会输出到stderr，其他信息输出到stdout。

## cat
使用方法：hadoop fs -cat URI [URI …]  
将路径指定文件的内容输出到stdout。  
示例：
> hadoop fs -cat hdfs://host1:port1/file1 hdfs://host2:port2/file2  
hadoop fs -cat file:///file3 /user/hadoop/file4  
返回值：  
成功返回0，失败返回-1。  
## cp  
使用方法：hadoop fs -cp URI [URI …] <dest>  
将文件从源路径复制到目标路径。这个命令允许有多个源路径，此时目标路径必须是一个目录。   
示例：  
> hadoop fs -cp /user/hadoop/file1 /user/hadoop/file2  
hadoop fs -cp /user/hadoop/file1 /user/hadoop/file2 /user/hadoop/dir  
返回值：  
成功返回0，失败返回-1。
## du
使用方法：hadoop fs -du URI [URI …]  
显示目录中所有文件的大小，或者当只指定一个文件时，显示此文件的大小。  
示例：   
> hadoop fs -du /user/hadoop/dir1 /user/hadoop/file1 hdfs://host:port/user/hadoop/dir1   
返回值：  
成功返回0，失败返回-1。   

**dus**  
使用方法：hadoop fs -dus <args>  
显示文件的大小。
## reference 
[adoop Shell命令](https://hadoop.apache.org/docs/r1.0.4/cn/hdfs_shell.html)
