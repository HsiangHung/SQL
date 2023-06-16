# This folder is for SQL solutions of Stanford DB5 courses (DB5_* ) and SQLzoo (zoo_*).


# Window Function 

NOTE, the follwoing window functions are not available in MySQL, though they are available in PostgreSQL, Hive, among others.
Reference: [SQL Window Functions](https://www.sqltutorial.org/sql-window-functions/)

### [FIRST_VALUE()](https://www.sqltutorial.org/sql-window-functions/sql-first_value/), [LAST_VALUE()](https://www.sqltutorial.org/sql-window-functions/sql-last_value/)
First example:
```SQL
SELECT
    first_name,
    last_name,
    salary,
    FIRST_VALUE (first_name) OVER (
        ORDER BY salary
    ) lowest_salary
FROM
    employees e;
```
second example
```SQL
SELECT
    first_name,
    last_name,
    department_name,
    salary,
    FIRST_VALUE (CONCAT(first_name,' ',last_name)) OVER (
        PARTITION BY department_name
        ORDER BY salary
    ) lowest_salary
FROM
    employees e
    INNER JOIN departments d 
        ON d.department_id = e.department_id;
```

### [LAG()](https://www.sqltutorial.org/sql-window-functions/sql-lag/)

For each employee_id, move all slaray record one row down. So the result shows first value is always NULL.

```SQL
SELECT 
	employee_id, 
	fiscal_year, 
	salary,
	LAG(salary) OVER (
		PARTITION BY employee_id 
		ORDER BY fiscal_year) previous_salary
FROM
	basic_pays;
```

### [LEAD()](https://www.sqltutorial.org/sql-window-functions/sql-lead/)

Move everything in hire_date one row ahead, so the last row shows NULL.

```SQL
SELECT 
	first_name,
	last_name, 
	hire_date, 
	LEAD(hire_date, 1) OVER (
		ORDER BY hire_date
	) AS next_hired
FROM 
	employees;
```

## Ranking window functions

### [Rank()](https://www.sqltutorial.org/sql-window-functions/sql-rank/)
```SQL
SELECT
	col,
	RANK() OVER (
		ORDER BY col
	) myrank
FROM
	t;
```
| col | myrank |
| --- | --- | 
| A | 1 |
| B | 2 |
| B | 2 |
| C | 4 |
| D | 5 |
| D | 5 |
| E | 7 |

### [PERCENT_RANK()](https://www.sqltutorial.org/sql-window-functions/sql-percent_rank/)
```SQL
SELECT
    first_name,
    last_name,
    salary,
    ROUND(
        PERCENT_RANK() OVER (
            ORDER BY salary
        ) 
    ,2) percentile_rank
FROM
    employees; 
```
