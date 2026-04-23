
## 一、行转列（多行转一行，聚合）
### 1. 使用 collect_list() 和 collect_set()
```
-- 基本语法：将多行合并为数组
SELECT 
    group_column,
    collect_list(value_column) as value_list,    -- 保留重复，保持顺序
    collect_set(value_column) as value_set       -- 去重，无序
FROM table_name
GROUP BY group_column;

-- 示例：将每个部门的员工合并为列表
SELECT 
    dept_id,
    collect_list(emp_name) as emp_list,
    collect_set(skill) as unique_skills
FROM employee_skills
GROUP BY dept_id;
```
### 2. 使用 concat_ws() 拼接为字符串
```
-- 将数组转为分隔符连接的字符串
SELECT 
    dept_id,
    concat_ws(',', collect_list(emp_name)) as emp_names_str,
    concat_ws('|', collect_set(skill)) as skills_str
FROM employee_skills
GROUP BY dept_id;

-- 直接聚合拼接（Hive 2.0+）
SELECT 
    dept_id,
    collect_list(emp_name) as emp_array,
    array_join(collect_list(emp_name), ',') as emp_string  -- 直接转为字符串
FROM employee_skills
GROUP BY dept_id;
```
### 3. 多列聚合为JSON/MAP
```
-- 聚合为结构体数组
SELECT 
    dept_id,
    collect_list(
        named_struct('name', emp_name, 'salary', salary, 'age', age)
    ) as emp_info_array
FROM employees
GROUP BY dept_id;

-- 聚合为MAP（需保证key唯一）
SELECT 
    dept_id,
    map_from_arrays(
        collect_list(emp_id),
        collect_list(emp_name)
    ) as emp_map
FROM employees
GROUP BY dept_id;
```

## 二、列转行（一行转多行，展开）
### 1. 使用 explode() 展开数组
```
-- 基本语法：将数组的每个元素转为一行
SELECT 
    id,
    exploded_value
FROM table_name
LATERAL VIEW explode(array_column) t AS exploded_value;

-- 示例：展开员工技能数组
-- 原数据：id | emp_name | skills
--         1 | 张三     | [Java, SQL, Python]
SELECT 
    id,
    emp_name,
    skill
FROM employee_skills
LATERAL VIEW explode(skills) t AS skill;

-- 结果：
-- id | emp_name | skill
-- 1  | 张三     | Java
-- 1  | 张三     | SQL
-- 1  | 张三     | Python
```
### 2. 使用 posexplode() 展开数组（带位置索引）
```
-- 保留元素在原数组中的位置
SELECT 
    id,
    emp_name,
    pos,
    skill
FROM employee_skills
LATERAL VIEW posexplode(skills) t AS pos, skill;

-- 结果：
-- id | emp_name | pos | skill
-- 1  | 张三     | 0   | Java
-- 1  | 张三     | 1   | SQL
-- 1  | 张三     | 2   | Python
```
### 3. 使用 inline() 展开结构体数组
```
-- 展开结构体数组，每个字段成为一列
CREATE TABLE student_scores (
    student_id INT,
    scores ARRAY<STRUCT<subject:STRING, score:INT>>
);

-- 插入数据
INSERT INTO student_scores VALUES
(1, array(
    named_struct('subject', 'Math', 'score', 90),
    named_struct('subject', 'English', 'score', 85)
));

-- 使用inline展开
SELECT 
    student_id,
    subject,
    score
FROM student_scores
LATERAL VIEW inline(scores) t AS subject, score;

-- 结果：
-- student_id | subject | score
-- 1          | Math    | 90
-- 1          | English | 85
```
### 4. 使用 explode() 展开MAP
```
-- 展开MAP的键值对
SELECT 
    id,
    map_key,
    map_value
FROM table_with_map
LATERAL VIEW explode(column_map) t AS map_key, map_value;
```
## 三、 常见问题与解决方案
### 问题1：数组为空时的处理
```
-- 使用OUTER LATERAL VIEW避免空数组丢失整行
SELECT 
    id,
    emp_name,
    COALESCE(skill, 'N/A') as skill
FROM employee_skills
LATERAL VIEW OUTER explode(skills) t AS skill;  -- OUTER保留原行
```
### 问题2：多数组同时explode
```
-- 错误：不能同时explode多个数组
-- SELECT * FROM table LATERAL VIEW explode(arr1) t1 AS v1 LATERAL VIEW explode(arr2) t2 AS v2;

-- 正确：使用posexplode或分别处理
SELECT 
    t1.id,
    t1.v1,
    t2.v2
FROM (
    SELECT id, posexplode(arr1) as (pos1, v1) FROM table
) t1
JOIN (
    SELECT id, posexplode(arr2) as (pos2, v2) FROM table
) t2 ON t1.id = t2.id AND t1.pos1 = t2.pos2;
```
### 问题3：数组元素过多导致OOM
```
-- 限制数组大小
SELECT 
    id,
    slice(collect_list(value), 1, 1000) as top_1000  -- 只保留前1000个
FROM source_table
GROUP BY id;
```
## 四、实用函数总结
### 行转列函数
|函数|	用途|	示例|
|:----:|:--:|:---:|
|collect_list()	|聚合为数组（保留重复）	|collect_list(name)|
|collect_set()|	聚合为数组（去重）	|collect_set(name)|
|concat_ws()|	拼接为字符串	|concat_ws(',', list)|
|map_from_arrays()|	创建MAP	|map_from_array(keys, values)|
|named_struct()|	创建结构体	|named_struct('a', col1, 'b', col2)|
### 列转行函数
|函数|	用途	|示例|
|-|:-:|--|
|explode()	|展开数组为多行	|explode(arr)|
|posexplode()|	展开数组（带位置）	|posexplode(arr)|
|inline()	|展开结构体数组	|inline(struct_array)|
|stack()	|多列转多行|	stack(n, col1_as, col1, ...)|
|json_tuple()|	解析JSON	|json_tuple(json_str, '$.a', '$.b')|

## 五、总结：
行转列：使用 collect_list()、collect_set() 配合 GROUP BY

列转行：使用 explode()、posexplode() 配合 LATERAL VIEW

选择依据：查询需求、数据量、后续处理流程

性能关键：控制数组大小、合理使用索引、避免重复计算
