---
date: 2024-07-09
title: leetcode-SQL学习笔记 2024
tags:
  - SQL
description: 摘要：本文主要介绍 leetcode-SQL 学习过程的关键知识等。
---


## 找到员工表里面工资第N高的员工信息
```sql
CREATE TABLE employees (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50),
  department VARCHAR(50),
  salary DECIMAL(10, 2)
);

SELECT * FROM employees

INSERT INTO employees (name, department, salary) VALUES
  ('Alice', 'Sales', 5000.00),
  ('Bob', 'Sales', 4500.00),
  ('Charlie', 'Marketing', 6000.00),
  ('David', 'Marketing', 5500.00),
  ('Eve', 'IT', 7000.00),
  ('Frank', 'IT', 6500.00);

```
###  存储过程
```
CREATE OR REPLACE PROCEDURE get_nth_highest_salary(INOUT n INTEGER, INOUT nth_salary DECIMAL(10, 2))
AS $$
BEGIN
  SELECT DISTINCT salary INTO nth_salary
  FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
  ) t
  WHERE rnk = n;
END;
$$ LANGUAGE plpgsql;

DO $$
DECLARE
  n INTEGER := 2;
  result DECIMAL(10, 2);
BEGIN
  CALL get_nth_highest_salary(n, result);
  RAISE NOTICE 'The % highest salary is %', n, result;
END $$;
```
### 视图
```
CREATE OR REPLACE VIEW salary_ranks AS
SELECT DISTINCT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
FROM employees;

SELECT salary
FROM salary_ranks
WHERE rnk = 2;
```
### 函数
```
CREATE OR REPLACE FUNCTION get_nth(n INTEGER) RETURNS DECIMAL(10, 2) AS $$
BEGIN
  RETURN (
    SELECT DISTINCT salary
    FROM (
      SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
      FROM employees
    ) t
    WHERE rnk = n
  );
END;
$$ LANGUAGE plpgsql;
```

### 调用
```
SELECT get_nth(2);


SELECT 'DROP FUNCTION IF EXISTS ' || quote_ident(proname) || '(' || oidvectortypes(proargtypes) || ');'
FROM pg_proc
WHERE pronamespace = (SELECT oid FROM pg_namespace WHERE nspname = 'public');

SELECT proname, proargtypes, prorettype 
FROM pg_proc 
WHERE pronamespace = (SELECT oid FROM pg_namespace WHERE nspname = 'public');

DO $$ 
DECLARE
  stmt TEXT;
BEGIN
  FOR stmt IN 
    SELECT 'DROP FUNCTION IF EXISTS ' || quote_ident(proname) || '(' || oidvectortypes(proargtypes) || ');'
    FROM pg_proc
    WHERE pronamespace = (SELECT oid FROM pg_namespace WHERE nspname = 'public')
  LOOP
    EXECUTE stmt;
  END LOOP;
END $$;


SELECT FORMAT('DROP FUNCTION %s(%s);'
             ,p.oid::regproc
             ,pg_get_function_identity_arguments(p.oid))
FROM pg_proc AS p 
INNER JOIN pg_namespace AS n 
	ON p.pronamespace = n.oid 
WHERE n.nspname NOT IN ('pg_catalog', 'information_schema');


SELECT FORMAT('DROP FUNCTION %s(%s);'
             ,p.oid::regproc
             ,pg_get_function_identity_arguments(p.oid)) as drop_function_scripts
  FROM pg_proc AS p 
 INNER JOIN pg_namespace AS n 
    ON p.pronamespace = n.oid 
 WHERE n.nspname NOT IN ('pg_catalog', 'information_schema');


DO $$
DECLARE
  func_rec record;
BEGIN
  FOR func_rec IN 
    SELECT proname
          ,pg_catalog.pg_get_function_identity_arguments(p.oid) as argument_data_types
      FROM pg_proc p
      JOIN pg_namespace n 
        ON p.pronamespace = n.oid
     WHERE n.nspname = 'tzq'
  LOOP
    RAISE NOTICE 'Deleting function: %', func_rec.proname || '(' || regexp_replace(func_rec.argument_data_types, '\sDEFAULT\s[^,]+', '', 'g') || ')';
    EXECUTE 'DROP FUNCTION IF EXISTS ' || func_rec.proname || '(' || regexp_replace(func_rec.argument_data_types, '\sDEFAULT\s[^,]+', '', 'g') || ');';
  END LOOP;
END;
$$;

```




<Comment />