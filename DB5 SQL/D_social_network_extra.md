# D. SQL Social Network Query Extra Exercise


### Q3: What is the average number of friends per student? (Your result should be just one number.) 
```SQL
SELECT AVG(t.num)
FROM (SELECT COUNT(f.ID2) as num
      FROM Friend f JOIN Highschooler h ON (h.ID = f.ID1) 
      GROUP BY f.ID1) as t
```
