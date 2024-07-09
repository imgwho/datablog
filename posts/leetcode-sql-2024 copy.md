---
date: 2024-07-09
title: SQL学习笔记 面试题记录
tags:
  - SQL
description: 摘要：本文主要介绍面试中遇到的 SQL题及解答。
---


## 如果要生成下列结果， 该如何写sql语句？
```sql
-- 1、表table如下：
--   mon      dep     yj
-- 一月份      01      10
-- 一月份      02      10
-- 一月份      03      5
-- 二月份      02      8
-- 二月份      04      9
-- 三月份      03      8

-- 如果要生成下列结果， 该如何写sql语句？
-- dep   一月份     二月份      三月份
-- 01      10        null        null
-- 02      10         8         null
-- 03      5         null         8
-- 04     null         9         null 

-- 创建表并插入数据
CREATE TABLE depart (
mon VARCHAR(10),
dep VARCHAR(10),
yj INTEGER
);

INSERT INTO depart (mon, dep, yj)
VALUES
('一月份', '01', 10),
('一月份', '02', 10),
('一月份', '03', 5),
('二月份', '02', 8),
('二月份', '04', 9),
('三月份', '03', 8);

-- 解法1
SELECT
t1.dep,
t1.yj AS "一月份",
t2.yj AS "二月份",
t3.yj AS "三月份"
FROM
(SELECT dep, yj FROM depart WHERE mon = '一月份') t1
LEFT JOIN (SELECT dep, yj FROM depart WHERE mon = '二月份') t2 ON t1.dep = t2.dep
LEFT JOIN (SELECT dep, yj FROM depart WHERE mon = '三月份') t3 ON t1.dep = t3.dep
UNION
SELECT
t2.dep,
NULL AS "一月份",
t2.yj AS "二月份",
NULL AS "三月份"
FROM
(SELECT dep, yj FROM depart WHERE mon = '二月份') t2
LEFT JOIN (SELECT dep, yj FROM depart WHERE mon = '一月份') t1 ON t2.dep = t1.dep
WHERE t1.dep IS NULL
UNION
SELECT
t3.dep,
NULL AS "一月份",
NULL AS "二月份",
t3.yj AS "三月份"
FROM
(SELECT dep, yj FROM depart WHERE mon = '三月份') t3
LEFT JOIN (SELECT dep, yj FROM depart WHERE mon IN ('一月份', '二月份')) t12 ON t3.dep = t12.dep
WHERE t12.dep IS NULL;

-- 解法2
SELECT
dep,
MAX(CASE WHEN mon = '一月份' THEN yj END) AS "一月份",
MAX(CASE WHEN mon = '二月份' THEN yj END) AS "二月份",
MAX(CASE WHEN mon = '三月份' THEN yj END) AS "三月份"
FROM (
SELECT dep, '一月份' AS mon, yj FROM depart WHERE mon = '一月份'
UNION ALL
SELECT dep, '二月份' AS mon, yj FROM depart WHERE mon = '二月份'
UNION ALL
SELECT dep, '三月份' AS mon, yj FROM depart WHERE mon = '三月份'
) AS t
GROUP BY
dep;

```

## 2、一个表table中的Id有多个记录，查询出有两条以上记录的id号（给出至少两种写法）
```
-- 创建示例表并插入数据
CREATE TABLE fruit (
Id INT,
Value VARCHAR(50)
);


INSERT INTO fruit (Id, Value)
VALUES
(1, 'Apple'),
(1, 'Apple'),
(1, 'Apple'),
(2, 'Banana'),
(3, 'Cherry'),
(3, 'Cherry'),
(4, 'Durian'),
(5, 'Elderberry'),
(5, 'Elderberry'),
(6, 'Fig'),
(7, 'Grape'),
(7, 'Grape'),
(7, 'Grape'),
(8, 'Honeydew'),
(9, 'Jackfruit'),
(10, 'Kiwi'),
(10, 'Kiwi');

-- 解法1
SELECT id FROM fruit
GROUP BY id
HAVING COUNT(*) > 2

-- 解法2
SELECT DISTINCT t1.Id
FROM fruit t1
WHERE EXISTS (
SELECT COUNT(*)
FROM fruit t2
WHERE t1.Id = t2.Id
HAVING COUNT(*) > 2
)

-- 解法3
SELECT DISTINCT id
FROM (SELECT id, COUNT(*) OVER (PARTITION BY id) AS COUNT
    FROM fruit
 )t
 WHERE COUNT > 2

-- 3、解释这三种解法的效率
-- 1)、这个查询首先需要对整个表进行一次全表扫描,将所有的行按照Id分组。
-- 然后,它需要对每个分组计算COUNT(*),以确定每个Id的出现次数。
-- 最后,它使用HAVING子句过滤出出现次数超过2的Id。
-- 这个查询的效率主要取决于表的大小和重复Id的数量。如果表很大,并且重复的Id很多,那么分组和计数的操作可能会比较耗时。但如果表上的Id列有索引,数据库可以使用索引来优化GROUP BY操作,从而提高查询的效率。

-- 2)、这个查询首先需要对整个表进行一次全表扫描,对每一行计算COUNT(*) OVER (PARTITION BY Id)。
-- 窗口函数会对表的所有行进行处理,即使很多行可能并不需要。这可能会导致不必要的计算开销。
-- 最后,外层查询需要对结果进行DISTINCT操作,以去除重复的Id。
-- 这个查询的效率主要取决于表的大小和窗口函数的计算开销。如果表很大,窗口函数的计算可能会比较耗时。并且,因为窗口函数会对所有行进行计算,即使很多行并不需要,所以可能会有一些不必要的开销。

-- 3)、对于外层查询的每一行,内层子查询都需要执行一次。这意味着,如果外层查询返回n行,内层查询就需要执行n次。
-- 内层查询使用COUNT(*)对每个Id进行计数,并使用HAVING子句过滤出现次数超过2的Id。
-- 如果内层查询的条件(t1.Id = t2.Id)能够使用索引,那么内层查询的效率会比较高。
-- 这个查询的效率主要取决于表的大小、外层查询返回的行数以及内层查询的执行效率。如果外层查询返回的行数很多,那么内层查询就需要执行很多次,这可能会比较耗时。但如果内层查询能够使用索引,那么它的执行效率会比较高。
```
## 4、表table如下：

```
-- name course  score
-- 张三 语文 81
-- 张三 数学 75
-- 张三 英语 68
-- 1） 写出每门科目前三的分数
-- 2） 写出平均分在90分以上的学生


-- 创建表
CREATE TABLE students (
name VARCHAR(255),
course  VARCHAR(255),
score INTEGER
);

-- 插入数据
INSERT INTO students (name, course , score)
VALUES
('张三', '语文', 81),
('张三', '数学', 75),
('张三', '英语', 68),
('李四', '语文', 76),
('李四', '数学', 90),
('李四', '英语', 99),
('王五', '语文', 81),
('王五', '数学', 100),
('王五', '英语', 100),
('甲', '语文', 99),
('甲', '数学', 79),
('甲', '英语', 87);
```


### 1） 写出每门科目前三的分数
```-- 解法1
SELECT s1.course,
  s1.score
FROM students s1
  JOIN students s2 ON s1.course = s2.course
  AND s1.score <= s2.score
GROUP BY s1.course,
  s1.score
HAVING COUNT(*) <= 3
ORDER BY s1.course,
  s1.score DESC

-- 解法2
SELECT course, score
FROM (
SELECT course, score,
(SELECT COUNT(*) FROM students s2
WHERE s2.course = s1.course AND s2.score >= s1.score) AS ranking
FROM students s1
) t
WHERE ranking <= 3
ORDER BY course, ranking;

-- 解法3
SELECT course,
  score
FROM (
    SELECT course,
      score,
      ROW_NUMBER() OVER (
        PARTITION BY course
        ORDER BY score DESC
      ) AS ranking
    FROM students
  ) t
WHERE ranking <= 3
ORDER BY course,
  ranking;

SELECT * FROM students
```

### 2） 写出平均分在90分以上的学生
```-- 解法1
SELECT name, AVG(score) FROM students
GROUP BY name
HAVING AVG(score) > 90

-- 解法2
SELECT DISTINCT name, avg_score
FROM (
SELECT name, AVG(score) OVER (PARTITION BY name) AS avg_score
FROM students
) t
WHERE avg_score > 90
```

## 5、表table如下：
```-- quarter         sales
-- Q1             150
-- Q2             300
-- Q3             120
-- Q4             380

-- 创建qsales表并插入数据
CREATE TABLE qsales (
quarter VARCHAR(2),
sales INT
);


INSERT INTO qsales (quarter, sales)
VALUES
('Q1', 150),
('Q2', 300),
('Q3', 120),
('Q4', 380);

SELECT * FROM qsales

-- 解法1
SELECT 
    quarter AS "季度累计",
    SUM(sales) OVER (ORDER BY quarter) AS "业绩"
FROM qsales

-- 解法2
SELECT
    quarter AS "季度累计",
    (SELECT SUM(sales) FROM qsales WHERE quarter <= t.quarter) AS "业绩"
FROM qsales t
ORDER BY quarter

-- 解法3
SELECT
    t1.quarter AS "季度累计",
    SUM(t2.sales) AS "业绩"
FROM qsales t1
JOIN qsales t2 ON t1.quarter >= t2.quarter
GROUP BY t1.quarter
ORDER BY t1.quarter;
```
## 6、登录表playerlogin如下：
```-- Vopenid        ddatetime          
-- 小a           2017060506         
-- 小a           2017060611         
-- 小b           2017060606         
-- 小b           2017060506         
-- 小c           2017060606         
-- 小c           2017060506         

-- 金币流水表moneyflow如下：
-- Vopenid        ddatetime          money    AddOrReduce（充值即增加，消费即减少）
-- 小a           2017060506         500         增加
-- 小a           2017060608         1000        减少
-- 小a           2017060612         1000        减少
-- 小b           2017060606         100         减少
-- 小d           2017060106         1050        减少



-- 创建playerlogin表
CREATE TABLE playerlogin (
    Vopenid VARCHAR(10),
    ddatetime VARCHAR(10)
);

-- 插入playerlogin数据
INSERT INTO playerlogin (Vopenid, ddatetime)
VALUES
    ('小a', '2017060506'),
    ('小a', '2017060611'),
    ('小b', '2017060606'),
    ('小b', '2017060506'),
    ('小c', '2017060606'),
    ('小c', '2017060506');

-- 创建moneyflow表
CREATE TABLE moneyflow (
    Vopenid VARCHAR(10),
    ddatetime VARCHAR(10),
    money INT,
    AddOrReduce VARCHAR(10)
);

-- 插入moneyflow数据
INSERT INTO moneyflow (Vopenid, ddatetime, money, AddOrReduce)
VALUES
    ('小a', '2017060506', 500, '增加'),
    ('小a', '2017060608', 1000, '减少'),
    ('小a', '2017060612', 1000, '减少'),
    ('小b', '2017060606', 100, '减少'),
    ('小d', '2017060106', 1050, '减少');
```

### 1）用sql写出6月6日，有过登录的人数，有过充值的人数，及充值金额（一条sql写出查询三个数值）
```
-- 解法1
SELECT
    (SELECT COUNT(DISTINCT Vopenid) FROM playerlogin WHERE ddatetime LIKE '20170606%') AS login_count,
    (SELECT COUNT(DISTINCT Vopenid) FROM moneyflow WHERE ddatetime LIKE '20170606%' AND AddOrReduce = '增加') AS recharge_count,
    (SELECT COALESCE(SUM(money), 0) FROM moneyflow WHERE ddatetime LIKE '20170606%' AND AddOrReduce = '增加') AS recharge_amount;

-- 解法2
SELECT
    COUNT(DISTINCT p.Vopenid) AS login_count,
    COUNT(DISTINCT CASE WHEN m.AddOrReduce = '增加' THEN m.Vopenid END) AS recharge_count,
    COALESCE(SUM(CASE WHEN m.AddOrReduce = '增加' THEN m.money END), 0) AS recharge_amount
FROM playerlogin p
LEFT JOIN moneyflow m ON p.Vopenid = m.Vopenid AND p.ddatetime LIKE '20170606%' AND m.ddatetime LIKE '20170606%'
WHERE p.ddatetime LIKE '20170606%';


-- 解法3
SELECT
    COUNT(DISTINCT Vopenid) AS login_count,
    SUM(CASE WHEN AddOrReduce = '增加' THEN 1 ELSE 0 END) AS recharge_count,
    SUM(CASE WHEN AddOrReduce = '增加' THEN money ELSE 0 END) AS recharge_amount
FROM (
    SELECT Vopenid, NULL AS money, NULL AS AddOrReduce
    FROM playerlogin
    WHERE ddatetime LIKE '20170606%'
    UNION ALL
    SELECT Vopenid, money, AddOrReduce
    FROM moneyflow
    WHERE ddatetime LIKE '20170606%'
) AS t;


-- 解法4
SELECT
    (SELECT COUNT(DISTINCT Vopenid) FROM playerlogin WHERE ddatetime LIKE '20170606%') AS login_count,
    (SELECT COUNT(DISTINCT Vopenid) FROM moneyflow WHERE ddatetime LIKE '20170606%' AND AddOrReduce = '增加') AS recharge_count,
    (SELECT CASE WHEN EXISTS (SELECT 1 FROM moneyflow WHERE ddatetime LIKE '20170606%' AND AddOrReduce = '增加') 
            THEN (SELECT SUM(money) FROM moneyflow WHERE ddatetime LIKE '20170606%' AND AddOrReduce = '增加')
            ELSE 0 END) AS recharge_amount;


```


### 2）求出游戏中所有的玩家数
```
-- 解法1
SELECT COUNT(DISTINCT Vopenid) AS total_players
FROM (
    SELECT Vopenid FROM playerlogin
    UNION
    SELECT Vopenid FROM moneyflow
) AS all_players;

-- 解法2
WITH all_players AS (
    SELECT Vopenid, ROW_NUMBER() OVER (PARTITION BY Vopenid) AS rn
    FROM (
        SELECT Vopenid FROM playerlogin
        UNION ALL
        SELECT Vopenid FROM moneyflow
    ) AS t
)
SELECT COUNT(*) AS total_players
FROM all_players
WHERE rn = 1;

```




<Comment />