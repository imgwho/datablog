---
date: 2020-07-01
title: SQL学习笔记
tags:
  - SQL
description: 摘要：本文主要介绍 SQL 学习过程的关键知识等。
---

## 2019-005 SQL小挑战第五期 标签和逗号
表中两列数据，一列是userid 一列是用户标签tag，标签类别总共10个，每个用户可以拥有不同的标签，不同标签之间用逗号间隔。
```sql
+--------+-----------------+
| userid | tag             |
+--------+-----------------+
|      1 | css,go          |
|      2 | mysql,sql,html  |
|      3 | css,spring,php  |
|      4 | css,java,go,sql |
|      5 | java,c          |
|      6 | c               |
+--------+-----------------+
```
1：输出每个标签的用户数？
2：谁拥有的标签最多？
3：那个标签拥有用户最多？
```sql
select userid,tag,LENGTH(tag)-LENGTH(REPLACE(tag,',',''))+1 from user_tag;

LENGTH(tag)  包括逗号的字符串长度  -  LENGTH(REPLACE(tag,',','') 将逗号替换为空字符的字符串长度
相减就是逗号个数，加1就是标签个数
                                   
+--------+-----------------+-------------------------------------------+
| userid | tag             | LENGTH(tag)-LENGTH(REPLACE(tag,',',''))+1 |
+--------+-----------------+-------------------------------------------+
|      1 | css,go          |                                         2 |
|      2 | mysql,sql,html  |                                         3 |
|      3 | css,spring,php  |                                         3 |
|      4 | css,java,go,sql |                                         4 |
|      5 | java,c          |                                         2 |
|      6 | c               |                                         1 |
+--------+-----------------+-------------------------------------------+
6 rows in set (0.00 sec)
```
```sql
SELECT
  a.*,
  LENGTH(a.tag) - LENGTH(REPLACE(a.tag, ',', '')) + 1,
  b.userid
FROM user_tag a
  LEFT JOIN user_tag b
    ON b.userid <= (LENGTH(a.tag) - LENGTH(REPLACE(a.tag, ',', '')) + 1);
    
userid是递增的,这里过滤出小于等于标签数的递增序列为接下来的操作准备

+--------+-----------------+-----------------------------------------------------+--------+
| userid | tag             | LENGTH(a.tag) - LENGTH(REPLACE(a.tag, ',', '')) + 1 | userid |
+--------+-----------------+-----------------------------------------------------+--------+
|      1 | css,go          |                                                   2 |      1 |
|      1 | css,go          |                                                   2 |      2 |
|      2 | mysql,sql,html  |                                                   3 |      1 |
|      2 | mysql,sql,html  |                                                   3 |      2 |
|      2 | mysql,sql,html  |                                                   3 |      3 |
|      3 | css,spring,php  |                                                   3 |      1 |
|      3 | css,spring,php  |                                                   3 |      2 |
|      3 | css,spring,php  |                                                   3 |      3 |
|      4 | css,java,go,sql |                                                   4 |      1 |
|      4 | css,java,go,sql |                                                   4 |      2 |
|      4 | css,java,go,sql |                                                   4 |      3 |
|      4 | css,java,go,sql |                                                   4 |      4 |
|      5 | java,c          |                                                   2 |      1 |
|      5 | java,c          |                                                   2 |      2 |
|      6 | c               |                                                   1 |      1 |
+--------+-----------------+-----------------------------------------------------+--------+
15 rows in set (0.11 sec)
```

```sql
SELECT
  a.userid,
  SUBSTRING_INDEX(a.tag, ',', b.userid) AS tag
FROM user_tag a
  LEFT JOIN user_tag b
    ON b.userid <= (LENGTH(a.tag) - LENGTH(REPLACE(a.tag, ',', '')) + 1);

字符串拆分:
SUBSTRING_INDEX（str, delim, count）
str	需要拆分的字符串
delim	分隔符，通过某字符进行拆分
count	当 count 为正数，取第 n 个分隔符之前的所有字符；
      当 count 为负数，取倒数第 n 个分隔符之后的所有字符。

根据userid确定SUBSTRING_INDEX切分位置,正数是保留左边结果
+--------+-----------------+
| userid | tag             |
+--------+-----------------+
|      1 | css             |
|      1 | css,go          |
|      2 | mysql           |
|      2 | mysql,sql       |
|      2 | mysql,sql,html  |
|      3 | css             |
|      3 | css,spring      |
|      3 | css,spring,php  |
|      4 | css             |
|      4 | css,java        |
|      4 | css,java,go     |
|      4 | css,java,go,sql |
|      5 | java            |
|      5 | java,c          |
|      6 | c               |
+--------+-----------------+
```

```sql
SELECT
  a.userid,
  SUBSTRING_INDEX(SUBSTRING_INDEX(a.tag, ',', b.userid), ',', -1) AS tag
FROM user_tag a
  LEFT JOIN user_tag b
    ON b.userid <= (LENGTH(a.tag) - LENGTH(REPLACE(a.tag, ',', '')) + 1);

再用SUBSTRING_INDEX取最后一个逗号后面的字符串
+--------+--------+
| userid | tag    |
+--------+--------+
|      1 | css    |
|      1 | go     |
|      2 | mysql  |
|      2 | sql    |
|      2 | html   |
|      3 | css    |
|      3 | spring |
|      3 | php    |
|      4 | css    |
|      4 | java   |
|      4 | go     |
|      4 | sql    |
|      5 | java   |
|      5 | c      |
|      6 | c      |
+--------+--------+
接下来按照题意操作即可

这里也可以直接用mysql自带的数据库
help_topic作为辅助

mysql> SELECT a.userid,SUBSTRING_INDEX(SUBSTRING_INDEX(a.tag,',',b.help_topic_id+1),',',-1) as tag  
    -> from user_tag a left join mysql.help_topic b 
    -> on b.help_topic_id < (LENGTH(a.tag)-LENGTH(REPLACE(a.tag,',',''))+1);
+--------+--------+
| userid | tag    |
+--------+--------+
|      1 | css    |
|      1 | go     |
|      2 | mysql  |
|      2 | sql    |
|      2 | html   |
|      3 | css    |
|      3 | spring |
|      3 | php    |
|      4 | css    |
|      4 | java   |
|      4 | go     |
|      4 | sql    |
|      5 | java   |
|      5 | c      |
|      6 | c      |
+--------+--------+
15 rows in set (0.01 sec)

```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/93504/1593568651205-760fbfaa-9924-48f4-95d8-fdef5ec5c863.png#align=left&display=inline&height=378&originHeight=378&originWidth=722&size=57333&status=done&style=none&width=722)
**注意！**mysql 8.0.20 for win10的mysql.help_topic中help_topic_id是从0开始，一共有685个序列，如果每个用户的标签数大于这个数，可以自己新建一个表，生成连续id字段作为辅助，另外可参考 [https://juejin.im/post/5e55156ee51d4527235b5fc6](https://juejin.im/post/5e55156ee51d4527235b5fc6)
## 2019-006 SQL小挑战第六期 车流量问题
某市交通局为了监控车流信息，新进了一批摄像头，安装在交通要道，用于检测日常车流信息，让我们看看怎么处理这些信息呢？
摄像头收集到信息经过简化后数据结构如下，id（摄像头编号），date（时间），car（车流）
```sql
+----+------------+------+
| id | date       | car  |
+----+------------+------+
|  1 | 2019-05-11 |   99 |
|  2 | 2019-05-12 |  123 |
|  3 | 2019-05-13 |   89 |
|  4 | 2019-05-14 |  234 |
|  5 | 2019-05-15 |   37 |
|  6 | 2019-05-16 |  367 |
|  7 | 2019-05-17 |  765 |
|  8 | 2019-05-18 |  276 |
|  9 | 2019-05-19 |   87 |
+----+------------+------+
```
1：哪天的车流量最高？
```sql
select * from traffic order by car desc limit 1;

+----+------------+------+
| id | date       | car  |
+----+------------+------+
|  7 | 2019-05-17 |  765 |
+----+------------+------+
```

2：找出高峰期时段，要求连续三天及以上，并且每天人流量均不少于100。
```sql
--第一步，添加countt辅助列记录车流量大于100的数据，小于100的标记为0，不小于的在上次值+1
SELECT 
    t.*, @count := IF(t.car >= 100, @count + 1, 0) AS countt
  FROM
    traffic t, (SELECT @count := 0) b
    
+----+------------+------+--------+
| id | date       | car  | countt |
+----+------------+------+--------+
|  1 | 2019-05-11 |   99 |      0 |
|  2 | 2019-05-12 |  123 |      1 |
|  3 | 2019-05-13 |   89 |      0 |
|  4 | 2019-05-14 |  234 |      1 |
|  5 | 2019-05-15 |   37 |      0 |
|  6 | 2019-05-16 |  367 |      1 |
|  7 | 2019-05-17 |  765 |      2 |
|  8 | 2019-05-18 |  276 |      3 |
|  9 | 2019-05-19 |   87 |      0 |
+----+------------+------+--------+

--第二步，将id倒序，立flag，找到countt大于等于3或者上一个flag为1并且countt不为0的数据，标记flag为1
SELECT 
  t1.*, @flag := IF((t1.countt >= 3 OR @flag = 1) AND t1.countt != 0, 1, 0) AS flag
FROM
  (SELECT 
    t.*, @count := IF(t.car >= 100, @count + 1, 0) AS countt
  FROM
    traffic t, (SELECT @count := 0) b) t1, (SELECT @flag := 0) c
    ORDER BY id DESC

+----+------------+------+--------+------+
| id | date       | car  | countt | flag |
+----+------------+------+--------+------+
|  9 | 2019-05-19 |   87 |      0 |    0 |
|  8 | 2019-05-18 |  276 |      3 |    1 |
|  7 | 2019-05-17 |  765 |      2 |    1 |
|  6 | 2019-05-16 |  367 |      1 |    1 |
|  5 | 2019-05-15 |   37 |      0 |    0 |
|  4 | 2019-05-14 |  234 |      1 |    0 |
|  3 | 2019-05-13 |   89 |      0 |    0 |
|  2 | 2019-05-12 |  123 |      1 |    0 |
|  1 | 2019-05-11 |   99 |      0 |    0 |
+----+------------+------+--------+------+
--最后查询flag为1的数据就行啦，记得把id顺序排列
SELECT id, DATE, car FROM(
SELECT 
  t1.*, @flag := IF((t1.countt >= 3 OR @flag = 1) AND t1.countt != 0, 1, 0) AS flag
FROM
  (SELECT 
    t.*, @count := IF(t.car >= 100, @count + 1, 0) AS countt
  FROM
    traffic t, (SELECT @count := 0) b) t1, (SELECT @flag := 0) c
      ORDER BY id DESC) result
		WHERE flag = 1
		ORDER BY id;

+----+------------+------+
| id | DATE       | car  |
+----+------------+------+
|  6 | 2019-05-16 |  367 |
|  7 | 2019-05-17 |  765 |
|  8 | 2019-05-18 |  276 |
+----+------------+------+
```

3：如果要求连续10天以上，并且每天人流量均不少于100。你上述的方法还可行吗？有更好的建议吗？
可行，只需要修改语句里对应的条件即可。
## 2019-007 SQL小挑战第七期 分数的计算
2：sql_competition数据库中分为：chiese，math,english三个表
3：非pointer学员可以根据表结构自行建表练习
chiese,math,english三个表分别记录的是学生的语文，数学，英语成绩，三个表结构相同，都只有id(学生编号）score（分数）两列。
题目为：
1.查找所有科目均及格（>=60分）的同学并列出其各科分数；
2.查找所有同学的总分（没有分数的科目按照0分计算）和总分名次，并按照名次进行排序；
3.查找三个学科分数的平均数，中位数和众数：

```sql
SELECT math.id, math.score, chinese.score, english.score
FROM math
         LEFT JOIN chinese ON math.id = chinese.id
         LEFT JOIN english ON math.id = english.id
WHERE math.score > 60
  AND chinese.score > 60
  AND english.score > 60;



SELECT t2.*, @rankId := @rankId + 1
FROM (
         SELECT id, SUM(score), @rankId := 0 AS total
         FROM (
                  SELECT id, score FROM chinese UNION SELECT id, score FROM math UNION SELECT id, score FROM english
              ) t1
         GROUP BY id
         ORDER BY total DESC
     ) t2;
```

#### title表结构
```
+--------+--------------------+------------+------------+
| emp_no | title              | from_date  | to_date    |
+--------+--------------------+------------+------------+
|  10001 | Senior Engineer    | 1986-06-26 | 9999-01-01 |
|  10002 | Staff              | 1996-08-03 | 9999-01-01 |
|  10003 | Senior Engineer    | 1995-12-03 | 9999-01-01 |
|  10004 | Engineer           | 1986-12-01 | 1995-12-01 |
|  10004 | Senior Engineer    | 1995-12-01 | 9999-01-01 |
|  10005 | Senior Staff       | 1996-09-12 | 9999-01-01 |
|  10005 | Staff              | 1989-09-12 | 1996-09-12 |
|  10006 | Senior Engineer    | 1990-08-05 | 9999-01-01 |
|  10007 | Senior Staff       | 1996-02-11 | 9999-01-01 |
|  10007 | Staff              | 1989-02-10 | 1996-02-11 |
|  10008 | Assistant Engineer | 1998-03-11 | 2000-07-31 |
|  10009 | Assistant Engineer | 1985-02-18 | 1990-02-18 |
|  10009 | Engineer           | 1990-02-18 | 1995-02-18 |
|  10009 | Senior Engineer    | 1995-02-18 | 9999-01-01 |
|  10010 | Engineer           | 1996-11-24 | 9999-01-01 |
|  10010 | Engineer           | 1996-11-24 | 9999-01-01 |
+--------+--------------------+------------+------------+
```

#### 从titles表获取按照title进行分组，每组个数大于等于2，给出title以及对应的数目t。注意对于重复的emp_no进行忽略。
```sql
SELECT
  title,
  COUNT(DISTINCT emp_no)
FROM titles
GROUP BY title
HAVING t >= 2;

+--------------------+---+
| title              | t |
+--------------------+---+
| Assistant Engineer | 2 |
| Engineer           | 3 |
| Senior Engineer    | 5 |
| Senior Staff       | 2 |
| Staff              | 3 |
+--------------------+---+
```
#### 查找employees表所有emp_no为奇数，且last_name不为Mary的员工信息，并按照hire_date逆序排列
```sql
SELECT
  *
FROM employees
WHERE (emp_no % 2 = 1
AND last_name NOT IN ('Mary'))
ORDER BY hire_date DESC;

+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10011 | 1953-11-07 | Mary       | Sluis     | F      | 1990-01-22 |
|  10005 | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12 |
|  10007 | 1957-05-23 | Tzvetan    | Zielinski | F      | 1989-02-10 |
|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
|  10009 | 1952-04-19 | Sumant     | Peac      | F      | 1985-02-18 |
+--------+------------+------------+-----------+--------+------------+
```
#### 统计出当前各个title类型对应的员工当前薪水对应的平均工资。结果给出title以及平均工资avg。
```sql
SELECT
  t.title,
  AVG(s.salary)
FROM titles t
INNER JOIN salaries s
  ON t.emp_no = s.emp_no
  AND t.to_date = '9999-01-01'
  AND s.to_date = '9999-01-01'
GROUP BY title;

+-----------------+---------------+
| title           | AVG(s.salary) |
+-----------------+---------------+
| Senior Engineer | 62409.2500    |
| Staff           | 72527.0000    |
| Senior Staff    | 91381.0000    |
+-----------------+---------------+
```
#### 获取当前（to_date='9999-01-01'）薪水第二多的员工的emp_no以及其对应的薪水salary
```sql
SELECT
  emp_no,
  salary
FROM salaries
WHERE to_date = '9999-01-01'
AND salary = (SELECT DISTINCT
    salary
  FROM salaries
  WHERE to_date = '9999-01-01'
  ORDER BY salary DESC LIMIT 1, 1
    
+--------+--------+
| emp_no | salary |
+--------+--------+
|  10001 |  88958 |
+--------+--------+
```
#### 查找当前薪水(to_date='9999-01-01')排名第二多的员工编号emp_no、薪水salary、last_name以及first_name，不准使用order by
```sql
SELECT
  employees.emp_no,
  salary,
  last_name,
  first_name
FROM employees
  INNER JOIN salaries
    ON employees.emp_no = salaries.emp_no
WHERE to_date = '9999-01-01'
AND salary = (SELECT
    MAX(salary)
  FROM salaries
  WHERE salary <> (SELECT
      MAX(salary)
    FROM salaries
    WHERE to_date = '9999-01-01')
  AND to_date = '9999-01-01');
  
+--------+--------+-----------+------------+
| emp_no | salary | last_name | first_name |
+--------+--------+-----------+------------+
|  10001 |  88958 | Facello   | Georgi     |
+--------+--------+-----------+------------+
```
#### 查找所有员工的last_name和first_name以及对应的dept_name，也包括暂时没有分配部门的员工
```sql
SELECT
  e.last_name,
  e.first_name,
  d.dept_name
FROM employees e
  LEFT JOIN dept_emp de
    ON e.emp_no = de.emp_no
  LEFT JOIN departments d
    ON de.dept_no = d.dept_no;
    
+-----------+------------+--------------------+
| last_name | first_name | dept_name          |
+-----------+------------+--------------------+
| Facello   | Georgi     | Marketing          |
| Simmel    | Bezalel    | Marketing          |
| Bamford   | Parto      | Production         |
| Koblick   | Chirstian  | Production         |
| Maliniak  | Kyoichi    | Human Resources    |
| Preusig   | Anneke     | Finance            |
| Zielinski | Tzvetan    | Development        |
| Kalloufi  | Saniya     | Development        |
| Peac      | Sumant     | Quality Management |
| Piveteau  | Duangkaew  | Development        |
| Piveteau  | Duangkaew  | Quality Management |
| Sluis     | Mary       | NULL               |
+-----------+------------+--------------------+
```
#### 查找员工编号emp_no为10001其自入职以来的薪水salary涨幅值growth 

```sql
SELECT ( 
(SELECT salary FROM salaries WHERE emp_no = 10001 ORDER BY to_date DESC LIMIT 1) -
(SELECT salary FROM salaries WHERE emp_no = 10001 ORDER BY to_date ASC LIMIT 1)
) AS growth

+--------+
| growth |
+--------+
|  28841 |
+--------+
```
#### 查找所有员工自入职以来的薪水涨幅情况，给出员工编号emp_no以及其对应的薪水涨幅growth，并按照growth进行升序

```sql
SELECT
  a.emp_no,
  (b.salary - c.salary) AS growth
FROM employees AS a
  INNER JOIN salaries AS b
    ON a.emp_no = b.emp_no
    AND b.to_date = '9999-01-01'
  INNER JOIN salaries AS c
    ON a.emp_no = c.emp_no
    AND a.hire_date = c.from_date
ORDER BY growth ASC;

+--------+--------+
| emp_no | growth |
+--------+--------+
|  10005 |  16464 |
|  10001 |  28841 |
|  10007 |  31346 |
|  10004 |  34003 |
+--------+--------+
```

#### 统计各个部门对应员工涨幅的次数总和，给出部门编码dept_no、部门名称dept_name以及次数sum

```sql
SELECT
  dm.dept_no,
  dm.dept_name,
  COUNT(*) sum
FROM departments dm,
     dept_emp de,
     salaries s
WHERE dm.dept_no = de.dept_no
AND de.emp_no = s.emp_no
GROUP BY dm.dept_no;

SELECT
  de.dept_no,
  dp.dept_name,
  COUNT(s.salary) AS sum
FROM (dept_emp AS de
  INNER JOIN salaries AS s
    ON de.emp_no = s.emp_no)
  INNER JOIN departments AS dp
    ON de.dept_no = dp.dept_no
GROUP BY de.dept_no;

+---------+--------------------+-----+
| dept_no | dept_name          | sum |
+---------+--------------------+-----+
| d001    | Marketing          |  23 |
| d004    | Production         |  23 |
| d003    | Human Resources    |  13 |
| d002    | Finance            |  12 |
| d005    | Development        |  17 |
| d006    | Quality Management |  10 |
+---------+--------------------+-----+
```
#### 对所有员工的当前(to_date='9999-01-01')薪水按照salary进行按照1-N的排名，相同salary并列且按照emp_no升序排列

```sql
SELECT
  s1.emp_no,
  s1.salary,
  COUNT(DISTINCT s2.salary)
FROM salaries s1,
     salaries s2
WHERE s1.to_date = '9999-01-01'
AND s2.to_date = '9999-01-01'
AND s1.salary <= s2.salary/*给定s1.salary求有多少个s2.salary大于它*/
GROUP BY s1.emp_no,
         s1.salary
ORDER BY s1.salary DESC, s1.emp_no ASC;

+--------+--------+---------------------------+
| emp_no | salary | COUNT(DISTINCT s2.salary) |
+--------+--------+---------------------------+
|  10005 |  94692 |                         1 |
|  10001 |  88958 |                         2 |
|  10007 |  88070 |                         3 |
|  10004 |  74057 |                         4 |
|  10002 |  72527 |                         5 |
|  10003 |  43311 |                         6 |
|  10006 |  43311 |                         6 |
+--------+--------+---------------------------+
```
#### 获取所有非manager员工当前的薪水情况，给出dept_no、emp_no以及salary ，当前表示to_date='9999-01-01'

```sql
SELECT
  de.dept_no dept_no,
  de.emp_no emp_no,
  salary
FROM dept_emp de,
     salaries s
WHERE s.to_date = '9999-01-01'
AND s.emp_no NOT IN (SELECT
    emp_no
  FROM dept_manager)
AND de.emp_no = s.emp_no
AND de.to_date = '9999-01-01';

+---------+--------+--------+
| dept_no | emp_no | salary |
+---------+--------+--------+
| d001    |  10001 |  88958 |
| d004    |  10003 |  43311 |
| d005    |  10007 |  88070 |
+---------+--------+--------+
```
#### 获取员工其当前的薪水比其manager当前薪水还高的相关信息，当前表示to_date='9999-01-01',结果第一列给出员工的emp_no，第二列给出其manager的manager_no，第三列给出该员工当前的薪水emp_salary,第四列给该员工对应的manager当前的薪水manager_salary

```sql
SELECT
  sem.emp_no AS emp_no,
  sdm.emp_no AS manager_no,
  sem.salary AS emp_salary,
  sdm.salary AS manager_salary
FROM (SELECT
         s.salary,
         s.emp_no,
         de.dept_no
       FROM salaries s
         INNER JOIN dept_emp de
           ON s.emp_no = de.emp_no
           AND s.to_date = '9999-01-01') AS sem,
     (SELECT
         s.salary,
         s.emp_no,
         dm.dept_no
       FROM salaries s
         INNER JOIN dept_manager dm
           ON s.emp_no = dm.emp_no
           AND s.to_date = '9999-01-01') AS sdm
WHERE sem.dept_no = sdm.dept_no
AND sem.salary > sdm.salary;

+--------+------------+------------+----------------+
| emp_no | manager_no | emp_salary | manager_salary |
+--------+------------+------------+----------------+
|  10001 |      10002 |      88958 |          72527 |
+--------+------------+------------+----------------+
```
#### 汇总各个部门当前员工的title类型的分配数目，结果给出部门编号dept_no、dept_name、其当前员工所有的title以及该类型title对应的数目count

```sql
SELECT
  de.dept_no,
  dp.dept_name,
  t.title,
  COUNT(t.title) AS count
FROM titles AS t
  INNER JOIN dept_emp AS de
    ON t.emp_no = de.emp_no
    AND de.to_date = '9999-01-01'
    AND t.to_date = '9999-01-01'
  INNER JOIN departments AS dp
    ON de.dept_no = dp.dept_no
GROUP BY de.dept_no,
         t.title;

本题的关键在于用 GROUP BY 同时对 de.dept_no 和 t.title 进行分组，具体思路如下：
1、先用 INNER JOIN 连接 dept_emp 与 salaries
根据测试数据添加限定条件 de.to_date = '9999-01-01' AND t.to_date = '9999-01-01'
即当前员工的当前头衔
2、再用 INNER JOIN 连接departments，限定条件为 de.dept_no = dp.dept_no，即部门编号相同
3、最后用 GROUP BY 同时对 de.dept_no 和 t.title 进行分组
用 COUNT(t.title) 统计相同部门下相同头衔的员工个数
+---------+--------------------+-----------------+-------+
| dept_no | dept_name          | title           | count |
+---------+--------------------+-----------------+-------+
| d001    | Marketing          | Senior Engineer |     1 |
| d001    | Marketing          | Staff           |     1 |
| d004    | Production         | Senior Engineer |     2 |
| d003    | Human Resources    | Senior Staff    |     1 |
| d002    | Finance            | Senior Engineer |     1 |
| d005    | Development        | Senior Staff    |     1 |
| d006    | Quality Management | Senior Engineer |     1 |
| d006    | Quality Management | Engineer        |     2 |
+---------+--------------------+-----------------+-------+
```
#### 给出每个员工每年薪水涨幅超过5000的员工编号emp_no、薪水变更开始日期from_date以及薪水涨幅值salary_growth，并按照salary_growth逆序排列。提示：在sqlite中获取datetime时间对应的年份函数为strftime('%Y', to_date)

```sql
SELECT s2.emp_no, s2.from_date, (s2.salary - s1.salary) AS salary_growth
FROM salaries AS s1, salaries AS s2
WHERE s1.emp_no = s2.emp_no 
AND salary_growth > 5000
AND (strftime("%Y",s2.to_date) - strftime("%Y",s1.to_date) = 1 
     OR strftime("%Y",s2.from_date) - strftime("%Y",s1.from_date) = 1 )
ORDER BY salary_growth DESC
```
#### 查找描述信息中包括robot的电影对应的分类名称以及电影数目，而且还需要该分类对应电影数量>=5部

```sql
SELECT c.name, COUNT(fc.film_id) FROM
 (select category_id, COUNT(film_id) AS category_num FROM
     film_category  GROUP BY category_id HAVING count(film_id)>=5) AS cc,
 film AS f, film_category AS fc, category AS c
WHERE  f.description LIKE '%robot%'
AND f.film_id = fc.film_id
AND c.category_id = fc.category_id
AND c.category_id=cc.category_id
```
#### 使用join查询方式找出没有分类的电影id以及名称

```sql
SELECT f.film_id, f.title FROM film f LEFT JOIN film_category fc
ON f.film_id = fc.film_id WHERE fc.category_id IS NULL
解题思路是运用 LEFT JOIN 连接两表，用 IS NULL 语句限定条件：

1、用 LEFT JOIN 连接 film 和 film_category，限定条件为 f.film_id = fc.film_id，
即连接电影 id 和电影分类 id，如果电影没有分类，则电影分类 id 显示 null

2、再用 WHERE 来限定条件 fc.category_id IS NULL 选出没分类的电影

/* 注意：最后一句若写成 ON f.film_id = fc.film_id AND fc.category_id IS NULL，
则意义变成左连接两表 film_id 相同的记录，且 film_category 原表中的 fc.category 的值为 null。
显然，原表中的 fc.category 的值恒不为 null，
因此（f.film_id = fc.film_id AND fc.category_id IS NULL）恒为 FALSE，
左连接后则只会显示 film 表的数据，而 film_category 表的数据全显示为 null */
+---------+-----------------------------+
| film_id | title                       |
+---------+-----------------------------+
|       1 | ACADEMY DINOSAUR            |
|       2 | ACE GOLDFINGER              |
|       3 | ADAPTATION HOLES            |
|       4 | AFFAIR PREJUDICE            |
|       5 | AFRICAN EGG                 |
|       6 | AGENT TRUMAN                |
|       7 | AIRPLANE SIERRA             |
|       8 | AIRPORT POLLOCK             |
|       9 | ALABAMA DEVIL               |
|      10 | ALADDIN CALENDAR            |
|      11 | ALAMO VIDEOTAPE             |
|      12 | ALASKA PHANTOM              |
|      13 | ALI FOREVER                 |
|      14 | ALICE FANTASIA              |
|      15 | ALIEN CENTER                |
|      16 | ALLEY EVOLUTION             |
|      17 | ALONE TRIP                  |
|      18 | ALTER VICTORY               |
|      19 | AMADEUS HOLY                |
|      20 | AMELIE HELLFIGHTERS         |
|      21 | AMERICAN CIRCUS             |
|      22 | AMISTAD MIDSUMMER           |
|      23 | ANACONDA CONFESSIONS        |
|      24 | ANALYZE HOOSIERS            |
|      25 | ANGELS LIFE                 |
|      26 | ANNIE IDENTITY              |
|      27 | ANONYMOUS HUMAN             |
|      28 | ANTHEM LUKE                 |
|      29 | ANTITRUST TOMATOES          |
|      30 | ANYTHING SAVANNAH           |
|      31 | APACHE DIVINE               |
|      32 | APOCALYPSE FLAMINGOS        |
|      33 | APOLLO TEEN                 |
|      34 | ARABIA DOGMA                |
|      35 | ARACHNOPHOBIA ROLLERCOASTER |
|      36 | ARGONAUTS TOWN              |
|      37 | ARIZONA BANG                |
|      38 | ARK RIDGEMONT               |
|      39 | ARMAGEDDON LOST             |
|      40 | ARMY FLINTSTONES            |
|      41 | ARSENIC INDEPENDENCE        |
|      42 | ARTIST COLDBLOODED          |
|      43 | ATLANTIS CAUSE              |
|      44 | ATTACKS HATE                |
|      45 | ATTRACTION NEWTON           |
|      46 | AUTUMN CROW                 |
|      47 | BABY HALL                   |
|      48 | BACKLASH UNDEFEATED         |
|      49 | BADMAN DAWN                 |
+---------+-----------------------------+
```
#### 使用子查询的方式找出属于Action分类的所有电影对应的title,description

```sql
SELECT
  f.title,
  f.description
FROM film AS f
WHERE f.film_id IN (SELECT
    fc.film_id
  FROM film_category AS fc
  WHERE fc.category_id IN (SELECT
      c.category_id
    FROM category AS c
    WHERE c.name = 'Action'));
```
#### 使用含有关键字exists查找未分配具体部门的员工的所有信息。

```sql
SELECT * FROM employees WHERE NOT EXISTS 
(SELECT emp_no FROM dept_emp WHERE emp_no = employees.emp_no)

+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10011 | 1953-11-07 | Mary       | Sluis     | F      | 1990-01-22 |
+--------+------------+------------+-----------+--------+------------+
```

#### 小数点数据库题

```sql
SELECT
	st.*,
	sc1.score,
	sc1.CId,
	sc2.score,
	sc2.CId 
FROM
	student st
	LEFT JOIN sc sc1 ON st.SId = sc1.SId
	LEFT JOIN sc sc2 ON sc1.SId = sc2.SId 
WHERE
	sc1.cid = '01' 
	AND sc2.cid = '02' 
	AND sc1.score > sc2.score;
```

#### 创建表

```sql
CREATE TABLE Product (
  product_id CHAR(4) NOT NULL PRIMARY KEY, 
  product_name VARCHAR(100) NOT NULL, 
  product_type VARCHAR(32) NOT NULL, 
  sale_price INTEGER, 
  purchase_price INTEGER, 
  regist_date DATE, 
  PRIMARY KEY (product_id)
);
```
 
#### 添加一列可以存储100位的可变长字符串的product_name_pinyin列

```sql
ALTER TABLE Product ADD COLUMN product_name_pinyin VARCHAR(100);
```

 
#### 删除product_name_pinyin列

```sql
ALTER TABLE Product DROP COLUMN product_name_pinyin;
```
 
#### 插入数据

```sql
START TRANSACTION;
INSERT INTO Product VALUES ('0001', 'T恤' ,'衣服', 1000, 500, '2009-09-20');
INSERT INTO Product VALUES ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11');
INSERT INTO Product VALUES ('0003', '运动T恤', '衣服', 4000, 2800, NULL);
INSERT INTO Product VALUES ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');
INSERT INTO Product VALUES ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
INSERT INTO Product VALUES ('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20');
INSERT INTO Product VALUES ('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28');
INSERT INTO Product VALUES ('0008', '圆珠笔', '办公用品', 100, NULL, '2009-11-11');
COMMIT;
```
 
#### 重命名表名

```sql
RENAME TABLE Poduct to Product;
```

#### 编写一条 SQL 语句，从 Product（商品）表中选取出“登记日期（ regist_ date）在 2009 年 4 月 28 日之后”的商品。查询结果要包含 product_ name 和 regist_date 两列。

```sql
SELECT product_name, regist_date
  FROM Product
 WHERE regist_date > '2009-04-28';
```
#### 从 Product 表中取出“销售单价（ sale_price）比进货单价（ purchase_price）高出 500日元以上”的商品。请写出两条可以得到相同结果的 SELECT 语句。

```sql
+--------------+------------+----------------+
| product_name | sale_price | purchase_price |
+--------------+------------+----------------+
| T恤          |       1000 |            500 |
| 运动T恤      |       4000 |           2800 |
| 高压锅       |       6800 |           5000 |
+--------------+------------+----------------+

SELECT product_name, sale_price, purchase_price
  FROM Product
 WHERE sale_price >= purchase_price + 500;


SELECT product_name, sale_price, purchase_price
  FROM Product
 WHERE sale_price - 500 >= purchase_price;
```
#### 请写出一条 SELECT 语句，从 Product 表中选取出满足“销售单价打九 折之后利润高于 100 日元的办公用品和厨房用具”条件的记录。查询结果 要包括 product_name 列、 product_type 列以及销售单价打九折之 后的利润（别名设定为 profit）。

```sql
SELECT
  p.product_name,
  p.product_type,
  p.sale_price * 0.9 - p.purchase_price AS profit
FROM product p
WHERE p.sale_price * 0.9 - p.purchase_price > 100
AND (p.product_type BETWEEN '办公用品' AND '厨房用具');

SELECT
  product_name,
  product_type,
  sale_price * 0.9 - purchase_price AS profit
FROM Product
WHERE sale_price * 0.9 - purchase_price > 100
AND (product_type = '办公用品'
OR product_type = '厨房用具');
```
#### 求出销售单价（ sale_price 列）合计值是 进货单价（ purchase_price 列）合计值 1.5 倍的商品种类。

```sql
SELECT product_type, SUM(sale_price), SUM(purchase_price)
  FROM Product
 GROUP BY product_type
HAVING SUM(sale_price) > SUM(purchase_price) * 1.5;

#having字句可以让我们筛选成组后的各种数据，where字句在聚合前先筛选记录，
#也就是说作用在group by和having字句前。而 having子句在聚合后对组记录进行筛选。

+--------------+-----------------+---------------------+
| product_type | SUM(sale_price) | SUM(purchase_price) |
+--------------+-----------------+---------------------+
| 衣服         | 5000            | 3300                |
| 办公用品     | 600             | 320                 |
+--------------+-----------------+---------------------+
```
#### 使用order by排序

```sql
SELECT *
  FROM Product
 ORDER BY sale_price, regist_date DESC;
 
+------------+--------------+--------------+------------+----------------+-------------+
| product_id | product_name | product_type | sale_price | purchase_price | regist_date |
+------------+--------------+--------------+------------+----------------+-------------+
| 0008       | 圆珠笔       | 办公用品     |        100 | NULL           | 2009-11-11  |
| 0006       | 叉子         | 厨房用具     |        500 | NULL           | 2009-09-20  |
| 0002       | 打孔器       | 办公用品     |        500 |            320 | 2009-09-11  |
| 0007       | 擦菜板       | 厨房用具     |        880 |            790 | 2008-04-28  |
| 0001       | T恤          | 衣服         |       1000 |            500 | 2009-09-20  |
| 0004       | 菜刀         | 厨房用具     |       3000 |           2800 | 2009-09-20  |
| 0003       | 运动T恤      | 衣服         |       4000 |           2800 | NULL        |
| 0005       | 高压锅       | 厨房用具     |       6800 |           5000 | 2009-01-15  |
+------------+--------------+--------------+------------+----------------+-------------+
```
#### 搜索case表达式
![image.png](https://cdn.nlark.com/yuque/0/2019/png/93504/1575963771588-fd166ded-404a-437e-889d-e2931f278a40.png#align=left&display=inline&height=708&originHeight=708&originWidth=752&size=289506&status=done&style=none&width=752)
#### 简单case表达式
![image.png](https://cdn.nlark.com/yuque/0/2019/png/93504/1575963794713-fe631f84-4878-4f70-9f46-a097b506038c.png#align=left&display=inline&height=573&originHeight=573&originWidth=622&size=145191&status=done&style=none&width=622)
#### case when 妙用
![image.png](https://cdn.nlark.com/yuque/0/2019/png/93504/1575964503655-69bc8833-5a97-4fa4-8681-47cf5ab5e865.png#align=left&display=inline&height=320&originHeight=320&originWidth=359&size=83802&status=done&style=none&width=359)
![image.png](https://cdn.nlark.com/yuque/0/2019/png/93504/1575964655248-fe288df0-1708-408b-a376-1cc81275cd5a.png#align=left&display=inline&height=565&originHeight=565&originWidth=484&size=148023&status=done&style=none&width=484)
![image.png](https://cdn.nlark.com/yuque/0/2019/png/93504/1575964770692-93cef8bc-95c1-499f-ba5c-6cbeecb8dfd7.png#align=left&display=inline&height=567&originHeight=567&originWidth=430&size=152715&status=done&style=none&width=430)
#### substr函数

- [`SUBSTR()`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substr) is a synonym for [`SUBSTRING()`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring).

- [`SUBSTRING(_`str`_,_`pos`_)`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring), [`SUBSTRING(_`str`_ FROM _`pos`_)`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring), [`SUBSTRING(_`str`_,_`pos`_,_`len`_)`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring), [`SUBSTRING(_`str`_ FROM _`pos`_ FOR _`len`_)`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_substring)
没有 len 参数的表单从位置 pos 开始的字符串 str 返回子字符串。带有 len 参数的表单从位置 pos 开始，从字符串字符串返回长的子字符串 len 字符。使用 FROM 的窗体是标准 SQL 语法。也可以对 pos 使用负值。在这种情况下，子字符串的开头是字符串末尾的 pos 字符，而不是开头。负值可用于此函数的任何形式的 pos。Pos 的值为 0 返回空字符串。对于所有形式的 SUBSTRING ()，要从中提取子字符串的字符串中第一个字符的位置被计算为 1。
```sql
mysql> SELECT SUBSTRING('Quadratically',5);
        -> 'ratically'
mysql> SELECT SUBSTRING('foobarbar' FROM 4);
        -> 'barbar'
mysql> SELECT SUBSTRING('Quadratically',5,6);
        -> 'ratica'
mysql> SELECT SUBSTRING('Sakila', -3);
        -> 'ila'
mysql> SELECT SUBSTRING('Sakila', -5, 3);
        -> 'aki'
mysql> SELECT SUBSTRING('Sakila' FROM -4 FOR 2);
        -> 'ki'
```

- 此函数是多字节安全的。如果 len 小于 1，则结果是空字符串。
#### ceil replace 函数
计算EMPLOYEES表中所有雇员的平均月薪后，意识到键盘的0键已损坏。发现自己的误算（使用除去了所有零的工资）与实际平均工资之间的区别。
编写一个查询来计算错误的数量（即：实际错误计算的平均月薪），并将其四舍五入到下一个整数。
错误表：
![](https://cdn.nlark.com/yuque/0/2019/png/93504/1576220693802-136f540b-6de1-4493-9eb1-2e5b2d1a4675.png#align=left&display=inline&height=225&originHeight=225&originWidth=318&size=0&status=done&style=none&width=318)
真实表：
![](https://cdn.nlark.com/yuque/0/2019/png/93504/1576220693523-92491002-9913-4eff-848b-21ba8bf129c4.png#align=left&display=inline&height=224&originHeight=224&originWidth=316&size=0&status=done&style=none&width=316)

计算的平均工资为98.00。实际平均工资是2159.00。
两次计算之间的最终误差为2159.00-98.00 = 2061.00，当四舍五入为下一个整数时，该误差为2061。

```sql
SELECT
  ceil(AVG(salary) - AVG(REPLACE(salary, '0', '')))
FROM employees;
```

ceil 函数：
返回不小于 X 的最小整数值。
```sql
mysql> SELECT CEILING(1.23);
        -> 2
mysql> SELECT CEILING(-1.23);
        -> -1
```
对于精确值数值参数，返回值具有精确值数值类型。对于字符串或浮点参数，返回值具有浮点类型。
replace函数：
[`REPLACE(_`str`_,_`from_str`_,_`to_str`_)`](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html#function_replace)
返回字符串 str，所有字符串都由字符串 from_str 替换。REPLACE () 在搜索 from_str 时执行区分大小写的匹配。
```sql
mysql> SELECT REPLACE('www.mysql.com', 'w', 'Ww');
        -> 'WwWwWw.mysql.com'
```
#### group by 1 分组
我们将员工的总收入定义为他们的月薪x工作月数，最大总收入定义为“员工”表中任何员工的最大总收入。编写查询以查找所有雇员的最大总收入以及拥有最大总收入的雇员总数。然后将这些值打印为2个以空格分隔的整数。
![](https://cdn.nlark.com/yuque/0/2019/png/93504/1576227239378-3c3d28a0-d20e-441d-ae25-de6353a4e79c.png#align=left&display=inline&height=398&originHeight=398&originWidth=348&size=0&status=done&style=none&width=348)

```sql
SELECT 
    (salary * months) AS earnings, COUNT(*)
FROM
    employee
GROUP BY 1
ORDER BY earnings DESC
LIMIT 1;
```
#### round函数
[`ROUND(_`X`_)`](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_round), [`ROUND(_`X`_,_`D`_)`](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_round)

将参数 X 舍入到 D 的小数位。舍入算法取决于 X 的数据类型。如果未指定，则默认为 0。D 可以是负数，以使值 X 的小数点左侧的 D 位变为零。

```sql
mysql> SELECT ROUND(-1.23);
        -> -1
mysql> SELECT ROUND(-1.58);
        -> -2
mysql> SELECT ROUND(1.58);
        -> 2
mysql> SELECT ROUND(1.298, 1);
        -> 1.3
mysql> SELECT ROUND(1.298, 0);
        -> 1
mysql> SELECT ROUND(23.298, -1);
        -> 20
```

返回值与第一个参数具有相同的类型 (假设它是整数、双或十进制)。这意味着对于整数参数，结果是一个整数 (没有小数位):
```sql
mysql> SELECT ROUND(150.000,2), ROUND(150,2);
+------------------+--------------+
| ROUND(150.000,2) | ROUND(150,2) |
+------------------+--------------+
|           150.00 |          150 |
+------------------+--------------+
```
ROUND () 根据第一个参数的类型使用以下规则:

- 对于精确值的数字，ROUND () 使用 “舍入一半远离零” 或 “舍入最近” 规则: 具有小数部分的值。如果是正数，则向上舍入到下一个整数，如果是负数，则向下舍入到下一个整数。(换句话说，它是从零舍入的。)小数部分小于.5 的值如果是正数，则舍入到下一个整数，如果是负数，则舍入到下一个整数。
- 对于近似值数字，结果取决于 C 库。在许多系统中，这意味着 ROUND () 使用 “舍入到最接近的偶数” 规则: 一个分数部分正好在两个整数中间的值被舍入到最接近的偶数整数。

以下示例显示舍入对于精确值和近似值的不同:
```sql
mysql> SELECT ROUND(2.5), ROUND(25E-1);
+------------+--------------+
| ROUND(2.5) | ROUND(25E-1) |
+------------+--------------+
| 3          |            2 |
+------------+--------------+
```
#### truncate 函数
[`TRUNCATE(_`X`_,_`D`_)`](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_truncate)

返回数字 X，截断为 D 小数位。如果 D 为 0，则结果没有小数点或小数部分。D 可以是负数，以使值 X 的小数点左侧的 D 位变为零。
```sql
mysql> SELECT TRUNCATE(1.223,1);
        -> 1.2
mysql> SELECT TRUNCATE(1.999,1);
        -> 1.9
mysql> SELECT TRUNCATE(1.999,0);
        -> 1
mysql> SELECT TRUNCATE(-1.999,1);
        -> -1.9
mysql> SELECT TRUNCATE(122,-2);
       -> 100
mysql> SELECT TRUNCATE(10.28*100,0);
       -> 1028
```
#### floor 函数
[`FLOOR(_`X`_)`](https://dev.mysql.com/doc/refman/8.0/en/mathematical-functions.html#function_floor)
返回不大于 X 的最大整数值。
```sql
mysql> SELECT FLOOR(1.23), FLOOR(-1.23);
        -> 1, -2
```





<Comment />