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
SELECT y.name, y.grade, z.name, z.grade
FROM Likes x JOIN Highschooler y ON (x.ID1 = y.ID)
             JOIN Highschooler z ON (x.ID2 = z.ID)
WHERE y.grade >= z.grade+2
```

### Q3: For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order.
```SQL
SELECT h1.name, h1.grade, h2.name, h2.grade
FROM Likes JOIN Highschooler h1 ON (Likes.ID1 = h1.ID)
           JOIN Highschooler h2 ON (Likes.ID2 = h2.ID)
WHERE Likes.ID1 IN
  (SELECT Likes.ID2
   FROM Likes)
   AND Likes.ID2 IN
  (SELECT Likes.ID1
   FROM Likes)
   AND h1.name < h2.name 
```
