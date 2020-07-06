## str_to_map
map<string,string>  
str_to_map(text[, delimiter1, delimiter2])

Splits text into key-value pairs using two delimiters. Delimiter1 separates text into K-V pairs, and Delimiter2 splits each K-V pair.  
Default delimiters are ',' for delimiter1 and '=' for delimiter2.

示例：  
```
select str_to_map('1:11&2:22', '&', ':')['1']

22
```

&nbsp;
## reference
[str_to_map hive 字符串转为map格式](https://blog.csdn.net/yuanyangsdo/article/details/64441165)
