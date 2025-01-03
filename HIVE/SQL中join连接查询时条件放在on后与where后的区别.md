<ul>
  <li> <p>可以这样理解&#xff1a;<strong>on是在生成连接表的起作用的&#xff0c;where是生成连接表之后对连接表再进行过滤。</strong></p> </li>
  <li> <p>当<strong>使用left join时&#xff0c;无论on的条件是否满足&#xff0c;都会返回左表的所有记录</strong><br /> 对于满足的条件的记录&#xff0c;两个表对应的记录会连接起来&#xff1b;对于不满足条件的记录&#xff0c;那右表字段全部是null</p> </li>
  <li> <p>当使用right join时&#xff0c;类似&#xff0c;只不过是全部返回右表的所有记录</p> </li>
  <li> <p>当使用inner join时&#xff0c;功能与where完全相同。</p> </li>
  <li> <p>分区表使用 <strong>非 inner join</strong>时&#xff0c;需要先各自用where限制分区再on、where。</p> </li>
</ul>
