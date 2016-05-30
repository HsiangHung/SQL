# D. SQL Social Network Query Extra Exercise


### Q3: What is the average number of friends per student? (Your result should be just one number.) 
```SQL
SELECT AVG(t.f)
FROM (SELECT ID1, COUNT(ID2) as f FROM Friend  GROUP BY Friend.ID1) as t
```
