### 1.分组查询每个班级的前三名
```
CREATE TABLE IF NOT EXISTS  student(
   name varchar(20),-- 姓名
   class varchar(20),-- 年级
   score int-- 分数
);
```
#### 实现分组查询每个班级的最高分
```
select a.class,
      a.score 
from student a 
inner join (
  select max(score) as score, 
         class as class 
  from student group by class) b
on a.class = b.class and a.score =b.score ;
```
#### 实现分组查询每个班级的前三名
```
select a.class,
       a.score 
from student a 
where (select count(*) from student where a.class=class and a.score<score)<3
order by a.class, a.score desc;
```
#### row_number() over()
```
select name,
      class,
      score
from(
   select name,
       class,
       score,
       row_number() over(partition by class order by score desc) rn
   from table)
where rn<4
```
