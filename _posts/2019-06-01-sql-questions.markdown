---
layout: post
title: SQL Interview Questions
categories: [development]
tags: [sql, question]
---

<br>SQL 知识点： 个人作为面试官的样题，主要针对校招/实习生，附上参考答案。

<br>

##### 样例表 log

```
id   user_id  time   url
1    001      xxx    A
2    001      xxx    B
3    002      xxx    A
4    002      xxx    C
...
...

```

<br>

##### 1. 查询昨天{ “访问量” | “访问人数” }最大的页面

```sql
(a) sql '排序'

SELECT url, {COUNT(*)|COUNT(DISTINCT user_id)} AS cnt
FROM log
GROUP BY url
WHERE time = 'xxx'
ORDER BY cnt DESC
LIMIT 1;


(b) sql '不排序'

WITH t1 AS (
  SELECT url, {COUNT(*)|COUNT(DISTINCT user_id)} AS cnt
  FROM log
  GROUP BY url
  WHERE time = 'xxx')
SELECT * FROM t1 WHERE cnt IN (SELECT MAX(cnt) FROM t1);


(c) python pandas

df[df.time == 'xxx'].groupby('url').apply(lambda x: pd.Series({
    'cnt' : {len(x)|len(x.user_id.unique())},
})).sort_values('cnt', ascending=False).iloc[0]

```

<br>

##### 2. 查询昨天作为 “入口页面” ，访问量在 100～500 间最大的页面（仅考虑作为入口页面的访问量）

```sql
(a) sql

WITH t1 AS (
  SELECT * FROM (
     SELECT user_id, time, url,
     ROW_NUMBER() OVER(PARTITION BY user_id ORDER BY time ASC) AS rank
     FROM log WHERE time = 'xxx')
  WHERE rank = 1 )
SELECT url, COUNT(*) AS cnt
FROM t1
GROUP BY url
HAVING cnt >= 100 AND cnt <= 500
ORDER BY cnt DESC
LIMIT 1;


(b) python pandas

df = df[df.time == 'xxx'].groupby('user_id'， as_index=False).apply(
  			lambda x: x.sort_values('time').iloc[0])
df = df.groupby('url').apply(lambda x: pd.Series({
    'cnt' : len(x),
}))
df[(df.cnt >= 100) & (df.cnt <= 500)].sort_values(
  'cnt', ascending=False).iloc[0]

```

<br>

##### 3. 思维扩展

```
这些数据可以如何分析，有什么可以挖掘的价值？ (可参考电商网站)

方案一：
对 url 分层，以电商网站为例，大体上分为“入口/门户页面”、商品详情页面、商品下单页面、商品付款页面，建立 Funnel 模型，可以从时间、商品类型、用户特征等纬度分析流失情况，优化销售流程；

方案二：
对商品分类，关联用户属性特征，从用户行为中推断用户兴趣，实现协同过滤和相关推荐；

方案三：
可以自由发挥想象。。。

```



<br>



<br>
<br>


