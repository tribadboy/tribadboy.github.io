---
layout: post
title: Solved Redshift Problems
categories: [development]
tags: [sql, redshift]
---

<br> Redshift 问题：在进行 ETL 过程中，检查数据仓库中是否缺失某日的数据。
<br> 思路： 构建连续日期序列，检索数据源日期，查看是否存在缺失。

<br>

##### 数仓中的样例表 ods_xxx.demo

```
id     time                     column    ...
...
101    2019-09-01 00:00:00      xxx       ...
102    2019-09-01 01:00:00      xxx       ...
103    2019-09-03 02:00:00      xxx       ...
104    2019-09-04 03:00:00      xxx       ...
...
...

```

<br>

##### 1. 常规方法：使用  `generate_series`

```sql
(a) '生成 2019-07-01 到今日的日期序列，再通过关联查询/子查询找到不存在的“日期”'

SELECT '2019-07-01'::date + i AS sys_date 
FROM generate_series(0, (now()::date - '2019-07-01'::date)) i;


```

>  该方法在 Redshift 上存在问题，Redshift 不完全支持 generate_series ；
>
>  generate_series 可以在 Redshift leader node 上执行，但无法在 compute node 上关联任何真实数据源，无法完成后续关联查询/子查询。

<br>

##### 2. 替代方法： 构建 `数字序列` 临时表

```sql
(b) '依次生成临时表 ten_numbers, generated_numbers, date_dimension'

WITH 
ten_numbers AS 
(
  SELECT 0 AS num UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 
    UNION SELECT 4 UNION SELECT 5 UNION SELECT 6 
    UNION SELECT 7 UNION SELECT 8 UNION SELECT 9
),
generated_numbers AS
(
  SELECT (1000*t1.num)+(100*t2.num)+(10*t3.num)+(t4.num) AS gen_num
  FROM ten_numbers AS t1
    JOIN ten_numbers AS t2 ON 1 = 1
    JOIN ten_numbers AS t3 ON 1 = 1
    JOIN ten_numbers AS t4 ON 1 = 1
),
date_dimension AS
(
  SELECT CURRENT_DATE - gen_num AS sys_date FROM generated_numbers
  WHERE gen_num >= 0 AND gen_num < 100
  ORDER BY sys_date DESC
)
SELECT * FROM date_dimension WHERE date_dimension.sys_date NOT IN 
(
  SELECT DISTINCT(DATE(time)) AS ori_date
  FROM ods_xxx.demo
)
ORDER BY sys_date;

```

<br>

##### 方法详解：

```
ten_numbers:          0 ~ 9 ， 数字构成的临时表

generated_numbers：   首先使用 'join xx on 1 = 1' ，
                      将四张 ten_numbers 进行笛卡尔乘积，
                      再对四列加权求和，得到 0 ～ 9999              
                      
date_dimension：      构建最近 100 天的日期序列

最后检索 ods_xxx.demo 的日期序列，查看近 100 天内是否存在缺失的日期


```



<br>

参考资料：
http://www.voidcn.com/article/p-qkrufdds-bwr.html


<br>
<br>


