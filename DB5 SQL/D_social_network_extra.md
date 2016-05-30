# D. SQL Social Network Query Extra Exercise


### Q3: What is the average number of friends per student? (Your result should be just one number.) 
```SQL
SELECT AVG(t.f)
FROM (SELECT ID1, COUNT(ID2) as f FROM Friend  GROUP BY Friend.ID1) as t
```


### Q4: Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend. 
```SQL
SELECT COUNT(h.name)
FROM Highschooler h JOIN Friend f ON (h.ID=f.ID1)
WHERE h.name != 'Cassandra' AND (
 f.ID2 IN 
(SELECT h.ID
 FROM Highschooler h
 WHERE h.name = ('Cassandra'))
 OR f.ID2 IN
(SELECT h.ID 
 FROM Highschooler h JOIN Friend f ON (h.ID=f.ID1)
 WHERE f.ID2 IN 
 (SELECT h.ID
  FROM Highschooler h
  WHERE h.name = ('Cassandra'))))
```
