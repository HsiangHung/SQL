# This folder is for SQL solutions of Stanford DB5 courses (DB5_* ) and SQLzoo (zoo_*).


# Window Function 

NOTE, the follwoing window functions are not available in MySQL, though they are available in PostgreSQL, Hive, among others.

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
