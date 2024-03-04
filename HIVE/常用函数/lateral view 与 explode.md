## 用法描述
**lateral view 要与UDTF函数一起使用，比如 explode() 函数。**    
这里的UDTF函数是指用户自定义的表生成函数（英文全称：user defined table-generating functions），它可以接受一行输入然后产生0行或多行输出。  
lateral view 首先将utdf函数应用到每一行上，这时每一行经utdf处理后得到多行输出，这些输出将会组建成一张虚拟表，然后这张虚拟表会跟当前表进行join操作，join完成之后会得出一张结果虚拟表，这张结果表里就有了utdf生成的列，当然原表的列除了utdf消耗的列之外肯定也在都里面。

## 1、split或str_to_map
由于explode()的输入要求array或map，若原始值为字符串需要先使用split()转换为array、或str_to_map()转为map  
### 1.1、split
<p><strong>语法:</strong> split(string str, string pat) <br /> <strong>返回值:</strong> array <br /> <strong>说明:</strong> 按照pat字符串分割str&#xff0c;会返回分割后的字符串数组 <br /> <strong>举例&#xff1a;</strong> <br /> 基本用法</p> 
<pre class="prettyprint"><code class=" hljs cs">hive&gt; <span class="hljs-keyword">select</span> split(<span class="hljs-string">&#39;abcdef&#39;</span>, <span class="hljs-string">&#39;c&#39;</span>) <span class="hljs-keyword">from</span> test;
[<span class="hljs-string">&#34;ab&#34;</span>, <span class="hljs-string">&#34;def&#34;</span>]</code></pre> 

### 1.2、str_to_map
str_to_map() 函数的第一个参数是要转换的字符串，第二个参数是键值对之间的分隔符（在这个例子中是逗号），第三个参数是键值对中键和值之间的分隔符（在这个例子中是冒号）。  
```
SELECT str_to_map('{"key1":"value1","key2":"value2"}',',',':') as my_map;
```

## 2、explode
***hive wiki对于expolde的解释如下：***  
**explode() takes in an array (or a map) as an input and outputs the elements of the array (map) as separate rows.   
UDTFs can be used in the SELECT expression list and as a part of LATERAL VIEW.**   
示例：  
> SELECT explode(myCol) AS myNewCol FROM myTable;  
*The usage with Maps is similar:*    
SELECT explode(myMap) AS (myMapKey, myMapValue) FROM myMapTable;  

**总结起来一句话：explode就是将hive一行中复杂的array或者map结构拆分成多行。**  

## 3、lateral view
***hive wiki 上的解释如下：***  
Lateral View Syntax  
lateralView: LATERAL VIEW udtf(expression) tableAlias AS columnAlias (‘,’ columnAlias)*   
fromClause: FROM baseTable (lateralView)*   
***Description***  
**Lateral view is used in conjunction with user-defined table generating functions such as *explode()*.   
As mentioned in Built-in Table-Generating Functions, a UDTF generates zero or more output rows for each input row.   
A lateral view first applies the UDTF to each row of base table and then joins resulting output rows to the input rows to form a virtual table having the supplied table alias.**

## 4、outer lateral view
later view 前面还可以加上一个 outer 关键字，这是为了避免 当udtf 没有得到任何结果时最终虚拟结果表里丢失原数据行的问题。具体来将，由于later view 的工作原理是将原表与 udtf 产生的虚拟表做 inner join 操作，所以如果 udtf 不产生任何结果时，那么对应原表的那一行也会在 inner join 操作后消失。outer关键字就是来解决这个问题的，加上这个关键字之后执行的就是 outer join 操作了，因此原表数据会被完全保留下来。


## 5、说明
Lateral View通常和UDTF一起出现，为了解决UDTF不允许在select字段的问题。   
Multiple Lateral View可以实现类似笛卡尔乘积。   
Outer关键字可以把不输出的UDTF的空结果，输出成NULL，防止丢失数据。  

## 6、示例
<p>考虑下面这张 <strong>pageAds</strong> 表：</p>
<table>
<thead>
<tr>
<th>Column name</th>
<th>Column type</th>
</tr>
</thead>
<tbody>
<tr>
<td>page_id</td>
<td>String</td>
</tr>
<tr>
<td>adid_list</td>
<td>Array&lt;int&gt;</td>
</tr>
</tbody>
</table>
<p>里面有两行数据：</p>
<table>
<thead>
<tr>
<th>page_id</th>
<th>adid_list</th>
</tr>
</thead>
<tbody>
<tr>
<td>front_page</td>
<td>[1,2,3]</td>
</tr>
<tr>
<td>contact_page</td>
<td>[3,4,5]</td>
</tr>
</tbody>
</table>
<p>这两行数据的意思是：front_page页有三条广告，id分别是1，2，3。contact_page页也有三条广告，id分别是3，4，5。<br>
现在需要解决的问题是：在所有页面中统计每条广告出现的次数，输出广告id和该广告出现的总次数。</p>
<p><strong>第一步</strong>：<br>
使用 lateral view 和 explore() 函数将 adid_list 列的 list 拆分，sql代码如下：</p>
<pre><code class="sql">SELECT page_id, ad_id
FROM pageAds LATERAL VIEW explode(adid_list) adTable AS ad_id;
</code></pre>
<p>可以得到以下结果：</p>
<table>
<thead>
<tr>
<th>page_id (string)</th>
<th>adid (int)</th>
</tr>
</thead>
<tbody>
<tr>
<td>"front_page"</td>
<td>1</td>
</tr>
<tr>
<td>"front_page"</td>
<td>2</td>
</tr>
<tr>
<td>"front_page"</td>
<td>3</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>3</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>4</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>5</td>
</tr>
</tbody>
</table>
<p>然后我们再使用 count/group by 语句统计出每个adid出现的次数：</p>
<pre><code class="sql">SELECT ad_id, count(1)
FROM pageAds LATERAL VIEW explode(adid_list) adTable AS ad_id
GROUP BY ad_id
</code></pre>
<p>得到最终结果：</p>
<table>
<thead>
<tr>
<th>ad_id</th>
<th>count(1)</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>1</td>
</tr>
<tr>
<td>2</td>
<td>1</td>
</tr>
<tr>
<td>3</td>
<td>2</td>
</tr>
<tr>
<td>4</td>
<td>1</td>
</tr>
<tr>
<td>5</td>
<td>1</td>
</tr>
</tbody>
</table>
<h3>outer lateral view</h3>
<p>later view 前面还可以加上一个 <strong>outer</strong> 关键字，这是为了避免 当udtf 没有得到任何结果时最终虚拟结果表里丢失原数据行的问题。具体来将，由于later view 的工作原理是将原表与 udtf 产生的虚拟表做 inner join 操作，所以如果 udtf 不产生任何结果时，那么对应原表的那一行也会在 inner join 操作后消失。<strong>outer</strong>关键字就是来解决这个问题的，加上这个关键字之后执行的就是 outer join 操作了，因此原表数据会被完全保留下来。</p>
<p>例如，上面的 pageAds 表中如果有以下三行数据：</p>
<table>
<thead>
<tr>
<th>page_id</th>
<th>adid_list</th>
</tr>
</thead>
<tbody>
<tr>
<td>front_page</td>
<td>[1,2,3]</td>
</tr>
<tr>
<td>contact_page</td>
<td>[3,4,5]</td>
</tr>
<tr>
<td>end_page</td>
<td>[ ]</td>
</tr>
</tbody>
</table>
<p>那么执行 lateral view 的sql：</p>
<pre><code class="sql">SELECT page_id, ad_id
FROM pageAds LATERAL VIEW explode(adid_list) adTable AS ad_id;
</code></pre>
<p>得到的结果将会是：</p>
<table>
<thead>
<tr>
<th>page_id (string)</th>
<th>adid (int)</th>
</tr>
</thead>
<tbody>
<tr>
<td>"front_page"</td>
<td>1</td>
</tr>
<tr>
<td>"front_page"</td>
<td>2</td>
</tr>
<tr>
<td>"front_page"</td>
<td>3</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>3</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>4</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>5</td>
</tr>
</tbody>
</table>
<p>可以看到，end_page 那一行消失了，因为它没有在 inner join 中关联到任何数据。因此如果想保留原表全部数据的话，就需要加上<strong>outer</strong>关键字：</p>
<pre><code class="sql">SELECT page_id, ad_id
FROM pageAds  OUTER LATERAL VIEW explode(adid_list) adTable AS ad_id;
</code></pre>
<p>得到如下结果</p>
<table>
<thead>
<tr>
<th>page_id (string)</th>
<th>adid (int)</th>
</tr>
</thead>
<tbody>
<tr>
<td>"front_page"</td>
<td>1</td>
</tr>
<tr>
<td>"front_page"</td>
<td>2</td>
</tr>
<tr>
<td>"front_page"</td>
<td>3</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>3</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>4</td>
</tr>
<tr>
<td>"contact_page"</td>
<td>5</td>
</tr>
<tr>
<td>“end_page”</td>
<td>&lt;NULL&gt;</td>
</tr>
</tbody>
</table>


## references  
[hive string转map](https://juejin.cn/s/hive%20string%E8%BD%ACmap)  
[Hive: lateral view 用法](https://www.jianshu.com/p/6098616b0f65)  
[hive lateral view 与 explode详解](https://blog.csdn.net/bitcarmanlee/article/details/51926530)  
[Hive Lateral view介绍](https://blog.csdn.net/youziguo/article/details/6837368)  
[Lateral View使用指南](https://blog.csdn.net/sunnyyoona/article/details/62894761)
