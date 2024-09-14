
<h3><a id="2_rlike_29"></a>1. rlike正则匹配</h3> 
<p>  可以通过<code>Java的正则表达式</code>来指定匹配条件<br />    <code>^ 符号表示匹配以给定模式开头的字符串&#xff0c;$ 符号表示匹配以给定模式结尾的字符串</code></p> 
<pre><code class="prism language-bash"><span class="token punctuation">[</span>a-z<span class="token punctuation">]</span> // 匹配所有的小写字母 
<span class="token punctuation">[</span>A-Z<span class="token punctuation">]</span> // 匹配所有的大写字母 
<span class="token punctuation">[</span>a-zA-Z<span class="token punctuation">]</span> // 匹配所有的字母 
<span class="token punctuation">[</span><span class="token number">0</span>-9<span class="token punctuation">]</span> // 匹配所有的数字 
<span class="token punctuation">[</span><span class="token number">0</span>-9<span class="token punctuation">\</span>.<span class="token punctuation">\</span>-<span class="token punctuation">]</span> // 匹配所有的数字&#xff0c;句号和减号 
<span class="token punctuation">[</span> <span class="token punctuation">\</span>f<span class="token punctuation">\</span>r<span class="token punctuation">\</span>t<span class="token punctuation">\</span>n<span class="token punctuation">]</span> // 匹配所有的白字符
</code></pre> 
<p>如&#xff1a;<br /> 查找名字中带有A的员工信息&#xff1a;</p> 
<pre><code class="prism language-bash"><span class="token keyword">select</span> * from emp where ename  RLIKE <span class="token string">&#39;[A]&#39;</span><span class="token punctuation">;</span>
</code></pre> 

```
 rlike  '.*(金条|足金|投资银|).*'
```

<h3><a id="1_like_11"></a>2. like语法匹配</h3> 
<p>  <code>% 代表零个或多个字符</code>(任意个字符)。<br />   <code>_ 代表一个字符</code></p> 
<p>如&#xff1a;<br /> 查找名字中第二个字母为A的员工信息&#xff1a;</p> 
<pre><code class="prism language-bash"><span class="token keyword">select</span> * from emp where ename LIKE <span class="token string">&#39;_A%&#39;</span><span class="token punctuation">;</span>
</code></pre> 

### 3、like与rlike的区别：
 like不是正则，而是通配符。这个通配符可以看一下SQL的标准，例如%代表任意多个字符。  
 rlike是正则，正则的写法与java一样。’\’需要使用’\\’,例如’\w’需要使用’\\w’

## references
[hive like语法、RLIKE正则匹配字符串](https://blog.csdn.net/javahelpyou/article/details/125548575)
