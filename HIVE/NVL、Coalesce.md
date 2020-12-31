## 一、NVL
### NVL（表达式1，表达式2）
如果表达式1为空值，NVL返回值为表达式2的值，否则返回表达式1的值。 

该函数的目的是把一个空值（null）转换成一个实际的值。其表达式的值可以是数字型、字符型和日期型。但是表达式1和表达式2的数据类型必须为同一个类型。

#### 例子
nvl(clue_num,0)：如果clue_num为空，则返回0；否则返回clue_num的值

### NVL2(表达式1，表达式2，表达式3）
如果表达式1为空，返回值为表达式3的值。如果表达式1不为空，返回值为表达式2的值。
#### 例子
nvl(sex,0,1):如果sex为空，则返回1；否则返回0

&nbsp;
## 二、Coalesce
Coalese函数的作用是的NVL的函数有点相似，其优势是有更多的选项。

### 格式如下：   
Coalesce(expr1, expr2, expr3….. exprn)
表示可以指定多个表达式的占位符。所有表达式必须是相同类型，或者可以隐性转换为相同的类型。

返回表达式中第一个非空表达式，如有以下语句： 　　SELECT COALESCE(NULL,NULL,3,4,5) FROM dual 　　其返回结果为：3

如果所有自变量均为 NULL，则 COALESCE 返回 NULL 值。 　　COALESCE(expression1,...n) 与此 CASE 函数等价：

#### 这个函数实际上是NVL的循环使用，在此就不举例子了。

## reference
[hive-NVL、Coalesce、NVL2、NULLIF函数](https://blog.csdn.net/qq_34941023/article/details/51440579)  
[SQL中NVL函数](https://www.cnblogs.com/as-zhlan/p/10717853.html)
