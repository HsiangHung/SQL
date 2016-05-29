# C. SQL Social Network Query Exercise

### Q1: Find the names of all students who are friends with someone named Gabriel.
```SQL
SELECT h.name 
FROM Highschooler h JOIN Friend f ON (h.ID = f.ID1)
WHERE f.ID2 in 
(SELECT Friend.ID2 
 FROM Friend JOIN Highschooler ON (Friend.ID2 = Highschooler.ID)
 WHERE Highschooler.name = ('Gabriel'))
```

### Q2: For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like.
```SQL
SELECT h.name, h.grade
FROM Highschooler h JOIN LIkes l ON (h.ID =l.ID1)
WHERE h.ID IN 
(SELECT Likes.ID2
 FROM Likes JOIN Highschooler ON (Likes.ID1 = Highschooler.ID)
 WHERE Highschooler.grade > 
  (SELECT Highschooler.grade +2
   FROM Highschooler
   )
)   
```
