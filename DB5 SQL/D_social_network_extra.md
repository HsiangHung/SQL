# D. SQL Social Network Query Extra Exercise

### Q1: For every situation where student A likes student B, but student B likes a different student C, return the names and grades of A, B, and C.
```SQL
SELECT a.name, a.grade, b.name, b.grade, c.name, c.grade
FROM 
(SELECT a.ID1, a.ID2, b.ID2 as ID3
FROM Likes a JOIN Likes b ON  (a.ID2 = b.ID1)
WHERE b.ID2 != a.ID1) as t  
JOIN Highschooler a ON (t.ID1 = a.ID)
JOIN Highschooler b ON (t.ID2 = b.ID)
JOIN Highschooler c ON (t.ID3 = c.ID)
```
The table "t" JOIN A->B->C and each of them join to highschooler tables.


### Q2: Find those students for whom all of their friends are in different grades from themselves. Return the students' names and grades.
```SQL
SELECT name, grade
FROM Highschooler
WHERE ID NOT IN (
   SELECT a.ID
   FROM Friend f JOIN Highschooler a ON (f.ID1 = a.ID)
              JOIN Highschooler b ON (f.ID2 = b.ID)
   WHERE a.grade = b.grade)
```
Note the query "SELECT a.ID ... WHERE a.grade = b.grade" gives the Friend tuple which both ID1 and ID2 are in the same grades.


### Q3: What is the average number of friends per student? (Your result should be just one number.) 
```SQL
SELECT AVG(t.f)
FROM (SELECT ID1, COUNT(ID2) as f FROM Friend  GROUP BY Friend.ID1) as t
```


### Q4: Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend. 
```SQL
SELECT COUNT(*)
FROM (SELECT DISTINCT h.ID
      FROM Highschooler h JOIN Friend ON (h.ID=Friend.ID1)
      WHERE Friend.ID2 IN (SELECT ID FROM Highschooler WHERE name ='Cassandra')
         OR Friend.ID1 IN (SELECT a.ID1
                     FROM Friend a JOIN Friend b ON (a.ID2 = b.ID1)
                     WHERE b.ID2 = (SELECT ID FROM Highschooler WHERE name ='Cassandra') 
                     AND b.ID2 != a.ID1) 
      AND Friend.ID1 NOT IN (SELECT ID FROM Highschooler WHERE name ='Cassandra') 
      )
```
Note! If the second row "SElECT DISTINCT h.ID" -> "SELECT DISTINCT h.name", then the answer is wrong. The reason there are two Gabriel.
The query
```SQL
SELECT a.ID1
FROM Friend a JOIN Friend b ON (a.ID2 = b.ID1)
WHERE b.ID2 = (SELECT ID FROM Highschooler WHERE name ='Cassandra') AND b.ID2 != a.ID1
```
gives the ID (=a.ID1) who's friends are Casandra's friends. Their common friends are a.ID2=b.ID1. "b.ID2 != a.ID1" removes Cassandra's friend is Cassandra.



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
