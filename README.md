# This folder is for SQL solutions of Stanford DB5 courses (DB5_* ) and SQLzoo (zoo_*).


# Window Function 

NOTE, the follwoing window functions are not available in MySQL, though they are available in PostgreSQL, Hive, among others.

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
