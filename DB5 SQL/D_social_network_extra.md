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

### Q5: Find the name and grade of the student(s) with the greatest number of friends. 
```SQL
SELECT h.name, h.grade
FROM Highschooler h JOIN Friend f ON (h.ID = f.ID1)
GROUP BY f.ID1
HAVING COUNT(f.ID2) = 
 (SELECT MAX(t.num)
  FROM (SELECT COUNT(f.ID2) as num
        FROM Friend f
        GROUP BY f.ID1) as t )
```
