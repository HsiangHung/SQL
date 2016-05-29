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
or much metter answer (using self join, and each join to students' roster):
```SQL
SELECT h.name, h.grade, m.name, m.grade
FROM Likes x JOIN Likes y ON (x.ID2 = y.ID1)
             JOIN Highschooler h ON (x.ID1 = h.ID)
             JOIN Highschooler m ON (y.ID1 = m.ID)
WHERE x.ID1 = y.ID2 and h.name < m.name
```

### Q4: Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade. 
```SQL
SELECT h.name, h.grade
FROM Highschooler h
WHERE h.ID NOT IN (SELECT Likes.ID1 FROM Likes)
   AND h.ID NOT IN (SELECT Likes.ID2 FROM Likes)
```

### Q5: 
For every situation where student A likes student B, but we have no information about whom B likes (that is, B does not appear as an ID1 in the Likes table), return A and B's names and grades. 
```SQL
SELECT h1.name, h1.grade, h2.name, h2.grade
FROM Likes l JOIN Highschooler h1 ON (l.ID1 = h1.ID)
             JOIN Highschooler h2 ON (l.ID2 = h2.ID)
WHERE l.ID2 NOT IN 
(SELECT Likes.ID1
 FROM Likes)
```

### Q6: Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade.
```SQL
SELECT DISTINCT h1.name, h1.grade/*, h2.name, h2.grade */
FROM Friend f JOIN Highschooler h1 ON (f.ID1 = h1.ID)
               JOIN Highschooler h2 ON (f.ID2 = h2.ID)
WHERE h1.ID NOT IN 
(SELECT h1.ID
 FROM Friend f JOIN Highschooler h1 ON (h1.ID = f.ID1)
               JOIN Highschooler h2 ON (h2.ID = f.ID2)
 WHERE h1.grade != h2.grade)
 ORDER BY h1.grade
```
