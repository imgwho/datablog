---
date: 2020-05-05
title: SQL学习笔记
tags:
  - tableau
  - dataviz-tips
description: 摘要：本文主要介绍 leetcode-SQL 学习过程的关键知识等。
---


## 175.组合两个表
```sql
表1: Person
+----------+-----------+----------+
| PersonId | FirstName | LastName |
+----------+-----------+----------+
|        1 | Allen     | Wang     |
+----------+-----------+----------+

表2: Address
+-----------+----------+---------------+----------+
| AddressId | PersonId | City          | State    |
+-----------+----------+---------------+----------+
|         1 |        2 | New York City | New York |
+-----------+----------+---------------+----------+
```

**编写一个 SQL 查询，满足条件：无论 person 是否有地址信息，都需要基于上述两表提供 person 的以下信息：**
**FirstName, LastName, City, State**
```sql
左连接即可

SELECT 
  firstname, lastname, city, state 
FROM
  person p 
  LEFT JOIN address ad
    ON p.personid = ad.personid 

+-----------+----------+------+-------+
| FirstName | LastName | City | State |
+-----------+----------+------+-------+
| Allen     | Wang     | NULL | NULL  |
+-----------+----------+------+-------+
```
## 
## 176. 第二高的薪水
```sql
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

**编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary）例如上述 Employee 表，SQL查询应该返回 200 ，作为第二高的薪水如果不存在第二高的薪水，那么查询应返回 null。**
```sql
SELECT
  IFNULL(
    (SELECT DISTINCT
      Salary
    FROM
      employee
    ORDER BY Salary
    LIMIT 1 OFFSET 1), NULL
  ) AS SecondHighestSalary 

+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

### [`IFNULL(_`expr1`_,_`expr2`_)`](https://dev.mysql.com/doc/refman/8.0/en/control-flow-functions.html#function_ifnull)
If _`expr1`_ is not `NULL`, [`IFNULL()`](https://dev.mysql.com/doc/refman/8.0/en/control-flow-functions.html#function_ifnull) returns _`expr1`_; otherwise it returns _`expr2`_.
```sql
mysql> SELECT IFNULL(1,0);
        -> 1
mysql> SELECT IFNULL(NULL,10);
        -> 10
mysql> SELECT IFNULL(1/0,10);
        -> 10
mysql> SELECT IFNULL(1/0,'yes');
        -> 'yes'
```

### LIMIT [offset,] rows | rows OFFSET offset
```sql
SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
```

Limit子句可以被用于强制 SELECT 语句返回指定的记录数。Limit接受一个或两个数字参数。参数必须是一个整数常量。如果给定两个参数，第一个参数指定第一个返回记录行的位置，第二个参数指定返回从那个记录开始往后的数量。
```sql
--初始记录行的偏移量是 0(而不是 1)：检索记录行6-15
mysql> SELECT * FROM table LIMIT 5,10;

--为了检索从某一个偏移量到记录集的结束所有的记录行，
--可以指定第二个参数为 -1：检索记录行 96-last
mysql> SELECT * FROM table LIMIT 95,-1;

--如果只给定一个参数，它表示返回最大的记录行数目。LIMIT n 等价于 LIMIT 0,n：
--检索前 5 个记录行
mysql> SELECT * FROM table LIMIT 5;
```

limit X offset Y = limit Y, X
从第Y条开始，选取X条
limit 1 offset 1
从第2条开始，选一条，所以就是排名第二的那条
limit X,Y
从第X条开始，选取Y条
limit X
选取前X条的记录


## 177.第n高的薪水
例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。
```sql
SELECT
  IFNULL(
    (SELECT DISTINCT
      Salary
    FROM
      employee
    ORDER BY Salary DESC
    LIMIT n ,1), NULL
  ) AS SecondHighestSalary 

如果用自定义函数则是
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
set N = N - 1;
  RETURN (
      select distinct salary from Employee order by salary desc limit N, 1
  );
END
```

## 178.分数排名
```sql
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```

**编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。**
```sql
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：

+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+

--自连接，找到表1小于表2的分数，数量，按照id分组，数量排序
--套路：group by order by   group by 能去重
SELECT 
  s1.Score, COUNT(DISTINCT (s2.Score)) rank1 
FROM
  scores s1 
  LEFT JOIN scores s2 
    ON s1.Score <= s2.Score 
+------+-------+------+-------+--------+
| Id   | Score | Id   | Score | result |
+------+-------+------+-------+--------+
|    1 |  3.50 |    1 |  3.50 |      4 |
+------+-------+------+-------+--------+
GROUP BY s1.Id 
+------+-------+------+-------+--------+
| Id   | Score | Id   | Score | result |
+------+-------+------+-------+--------+
|    1 |  3.50 |    1 |  3.50 |      4 |
|    2 |  3.65 |    2 |  3.65 |      3 |
|    3 |  4.00 |    3 |  4.00 |      1 |
|    4 |  3.85 |    3 |  4.00 |      2 |
|    5 |  4.00 |    3 |  4.00 |      1 |
|    6 |  3.65 |    2 |  3.65 |      3 |
+------+-------+------+-------+--------+
ORDER BY rank1 
+------+-------+------+-------+--------+
| Id   | Score | Id   | Score | result |
+------+-------+------+-------+--------+
|    3 |  4.00 |    3 |  4.00 |      1 |
|    5 |  4.00 |    3 |  4.00 |      1 |
|    4 |  3.85 |    3 |  4.00 |      2 |
|    2 |  3.65 |    2 |  3.65 |      3 |
|    6 |  3.65 |    2 |  3.65 |      3 |
|    1 |  3.50 |    1 |  3.50 |      4 |
+------+-------+------+-------+--------+

--或者用窗口函数
SELECT 
  Score, dense_rank () over (
ORDER BY Score DESC) 
FROM
  Scores ;

+-------+-------------------------------------------+
| Score | dense_rank () over (ORDER BY Score DESC) |
+-------+-------------------------------------------+
|  4.00 |                                         1 |
|  4.00 |                                         1 |
|  3.85 |                                         2 |
|  3.65 |                                         3 |
|  3.65 |                                         3 |
|  3.50 |                                         4 |
+-------+-------------------------------------------+
```

## 180. 连续出现的数字
```sql
+------+------+
| Id   | Num  |
+------+------+
|    1 |    1 |
|    2 |    1 |
|    3 |    1 |
|    4 |    2 |
|    5 |    1 |
|    6 |    2 |
|    7 |    2 |
+------+------+

```

**编写一个 SQL 查询，查找所有至少连续出现三次的数字。例如，给定上面的 `Logs` 表， `1` 是唯一连续出现至少三次的数字。**
```sql
SELECT DISTINCT a.Num ConsecutiveNums FROM 
(
SELECT t.Num ,
       @cnt:=IF(@pre=t.Num, @cnt+1, 1) cnt,
  --如果pre与t.num 相等，则cnt加1， 否则cnt重置为1
       @pre:=t.Num pre
  FROM Logs t, (SELECT @pre:=null, @cnt:=0) b
) a
  --变量初始化,pre记录上一个值,cnt统计次数
  WHERE a.cnt >= 3

+-----------------+
| ConsecutiveNums |
+-----------------+
|               1 |
+-----------------+
```

## 181.超过经理收入的员工
```sql
Employee 表包含所有员工，他们的经理也属于员工。
每个员工都有一个 Id，此外还有一列对应员工的经理的 Id。
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

**给定 `Employee` 表，编写一个 SQL 查询，该查询可以获取收入超过他们经理的员工的姓名。在上面的表格中，Joe 是唯一一个收入超过他的经理的员工。**
```sql
SELECT 
  e2.name Employee 
FROM
  employee e1 
  LEFT JOIN employee e2 
    ON e1.id = e2.ManagerId 
--只有员工表才有manager，所以才会有managerid，所以e1是员工表，e2是经理表

+------+-------+--------+-----------+------+-------+--------+-----------+
| Id   | Name  | Salary | ManagerId | Id   | Name  | Salary | ManagerId |
+------+-------+--------+-----------+------+-------+--------+-----------+
|    3 | Sam   |  60000 |      NULL |    1 | Joe   |  70000 |         3 |
|    4 | Max   |  90000 |      NULL |    2 | Henry |  80000 |         4 |
|    1 | Joe   |  70000 |         3 | NULL | NULL  |   NULL |      NULL |
|    2 | Henry |  80000 |         4 | NULL | NULL  |   NULL |      NULL |
+------+-------+--------+-----------+------+-------+--------+-----------+
WHERE e1.Salary < e2.Salary ;
--AND e1.Salary < e2.Salary ;

+----------+
| Employee |
+----------+
| Joe      |
+----------+
```

## 182. 查找重复的电子邮箱
```sql
+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
```

**编写一个 SQL 查询，查找 Person 表中所有重复的电子邮箱。**
```sql
SELECT 
  email 
FROM
  person 
GROUP BY email 
HAVING COUNT(email) > 1;

+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

## 183. 从不订购的客户
```sql
Customers 表：
+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+

Orders 表：
+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
```

**某网站包含两个表，`Customers` 表和 `Orders` 表。编写一个 SQL 查询，找出所有从不订购任何东西的客户。**
```sql
SELECT NAME 
FROM
  customers c 
  LEFT JOIN orders o 
    ON c.Id = o.CustomerId 
--自连接，找到order表里为空的记录，就是从不订购的客户
WHERE o.id IS NULL ;


+-------+
| NAME  |
+-------+
| Henry |
| Max   |
+-------+
```

## 184.部门工资最高的员工
```sql
Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
Department 表包含公司所有部门的信息。

+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

**编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。**
```sql
SELECT 
  dp.Name Department, e.Name Employee, Salary 
FROM
  employee e 
  LEFT JOIN department dp 
    ON e.DepartmentId = dp.id 
WHERE (e.DepartmentId, Salary) IN 
-- where筛选id, salary 符合条件的数据
  (SELECT 
    DepartmentId, MAX(Salary) 
  FROM
    employee 
  GROUP BY DepartmentId);
  
+-------+-------+--------+
| Name  | Name  | Salary |
+-------+-------+--------+
| IT    | Jim   |  90000 |
| IT    | Max   |  90000 |
| Sales | Henry |  80000 |
+-------+-------+--------+
```

## 185.求部门工资前三高的所有员工
```sql
SELECT 
  DepartmentId, NAME, Salary 
FROM
  (SELECT 
    *, row_number () over (
      PARTITION BY DepartmentId 
  ORDER BY Salary DESC
  ) AS ranking 
  FROM
    Employee) AS a 
WHERE ranking <= 3 ;


SELECT 
  d.Name Department, c.name Employee, c.Salary 
FROM
  (SELECT 
    e1.*, COUNT(DISTINCT e2.Salary) num 
  FROM
    Employee e1 
    LEFT JOIN Employee e2 
      ON e1.Salary < e2.Salary 
      AND e1.DepartmentId = e2.DepartmentId 
  GROUP BY e1.name) c, Department d 
WHERE c.DepartmentId = d.id 
  AND num < 3 ;


+--------------+-------+--------+
| DepartmentId | NAME  | Salary |
+--------------+-------+--------+
|            1 | Jim   |  90000 |
|            1 | Max   |  90000 |
|            1 | Joe   |  70000 |
|            2 | Henry |  80000 |
|            2 | Sam   |  60000 |
+--------------+-------+--------+
```

## 196.删除重复的电子邮箱
```sql
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id 是这个表的主键。

```

**编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。**
```sql
DELETE 
  P1 
FROM
  Person AS P1 
  JOIN Person AS P2 
    ON (
      P1.email = P2.email 
      AND P1.id > P2.id
    )
```

## 197.上升的温度
```sql
+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+
```

**给定一个 Weather 表，编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 Id。**
```sql
SELECT 
  w2.id 
FROM
  Weather w1 
  JOIN Weather w2 
    ON w2.Temperature > w1.Temperature 
WHERE DATEDIFF(w2.RecordDate, w1.RecordDate) = 1 

+------+
| id   |
+------+
|    2 |
|    4 |
+------+
```

## 262.行程和用户
```sql
Trips 表中存所有出租车的行程信息。每段行程有唯一键 Id，
Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。
Status 是枚举类型，
枚举成员为 (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’)。
+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+

Users 表存所有用户。每个用户有唯一键 Users_Id。
Banned 表示这个用户是否被禁止，
Role 则是一个表示（‘client’, ‘driver’, ‘partner’）的枚举类型。
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
```

**写一段 SQL 语句查出 2013年10月1日 至 2013年10月3日 期间非禁止用户的取消率。基于上表，你的 SQL 语句应返回如下结果，取消率（Cancellation Rate）保留两位小数。取消率的计算方式如下：(被司机或乘客取消的非禁止用户生成的订单数量) / (非禁止用户生成的订单总数)**
```sql
SELECT 
  t.request_at DAY, (
    ROUND(
      COUNT(
        IF(STATUS != 'completed', STATUS, NULL)
      ) / COUNT(STATUS), 2
    )
  ) AS 'Cancellation Rate' 
FROM
  Users u 
  INNER JOIN Trips t 
    ON u.Users_id = t.Client_Id 
    AND u.banned != 'Yes' 
WHERE t.Request_at >= '2013-10-01' 
  AND t.Request_at <= '2013-10-03' 
GROUP BY t.Request_at;

+------------+-------------------+
| DAY        | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |              0.33 |
| 2013-10-02 |              0.00 |
| 2013-10-03 |              0.50 |
+------------+-------------------+
```




<Comment />