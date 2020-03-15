## 创建目录
```
hdfs dfs -mkdir /test
```
加 -p 则创建多级目录，如同linux中的mkidr –p。
```
hdfs dfs -mkdir -p /test/file
```
## 将本地文件或目录（h.csv)上传到HDFS中的路径( /usr/file)
```
hdfs dfs -put h.csv /test/file
```
## 将文件或目录从HDFS中的路径(/test/file/h.csv)拷贝到本地文件路径(/)
```
hdfs dfs -get /test/file/h.csv /
```
## 查看目录下内容，包括文件名，权限，所有者，大小和修改时间
```
[apache@namenode01 ~]$ hdfs dfs -ls /test/file
Found 1 items
-rw-r--r--   2 apache supergroup     665937 2018-09-26 16:14 /test/file/h.csv
```
## 与ls相似，但递归地显示子目录下的内容
```
[apache@namenode01 ~]$ hdfs dfs -ls -R /test
drwxr-xr-x   - apache supergroup          0 2018-09-26 16:14 /test/file
-rw-r--r--   2 apache supergroup     665937 2018-09-26 16:14 /test/file/h.csv
```
## 显示path下所有文件磁盘使用情况下，用字节大小表示，文件名用完整的HDFS协议前缀表示
```
[apache@namenode01 ~]$ hdfs dfs -du -h  /test/file
650.3 K  /test/file/h.csv
```
## df与-du相似，但它还显示全部文件或目录磁盘使用情况
```
[apache@namenode01 ~]$ hdfs dfs -df -h  /test/file
Filesystem            Size     Used  Available  Use%
hdfs://dev-cluster  49.2 T  443.7 G     46.3 T    1%
```
## 在HDFS中，将文件或目录从HDFS的源路径移动到目标路径
```
[apache@namenode01 ~]$ hdfs dfs -mv -R /test/file/h.csv /test
```
## 在HDFS中，将/test/file/h.csv文件或目录复制到/test
```
[apache@namenode01 ~]$ hdfs dfs -cp -R /test/file/h.csv /test
```
## reference
[HDFS基本命令](https://blog.csdn.net/sunbocong/article/details/82855506)
