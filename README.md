# This folder is for SQL solutions of Stanford DB5 courses (DB5_* ) and SQLzoo (zoo_*).


# Window Function 

NOTE, the follwoing window functions are not available in MySQL, though they are available in PostgreSQL, Hive, among others.

## Ranking window functions

```SQL
SELECT
	col,
	RANK() OVER (
		ORDER BY col
	) myrank
FROM
	t;
```
