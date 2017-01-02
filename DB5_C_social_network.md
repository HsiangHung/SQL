# C. SQL Social Network Query Exercise

### Note: the friendship in Friend is bidirectionl, but like is one-directional.

#### Warmup-1: Find all students who like others who like back; A likes B and B also likes A.
```SQL
select l1.ID1
from Likes l1 join Likes l2 on (l1.ID2 = l2.ID1)
where l1.ID1 = l2.ID2
```

#### Warmup-2: Find all students who like others but they don't like back; A likes B but no B likes A.
```SQL
select ID1
from Likes
where ID1 not in (
select l1.ID1
from Likes l1 join Likes l2 on (l1.ID2 = l2.ID1)
where l1.ID1 = l2.ID2)
```

#### Warmup-3: Find all students who like others but they are not friends; A likes B but B is not A's friend.
```SQL
select l.ID1, l.ID2
from Likes l
where l.ID2 not in (select f.ID2 from Friend f where l.ID1=f.ID1)
```


#### Q1: Find the names of all students who are friends with someone named Gabriel.
```SQL
SELECT h.name 
FROM Highschooler h JOIN Friend f ON (h.ID = f.ID1)
WHERE f.ID2 in 
(SELECT Friend.ID2 
 FROM Friend JOIN Highschooler ON (Friend.ID2 = Highschooler.ID)
 WHERE Highschooler.name = ('Gabriel'))
```
alternative solution:
```SQL
select h.name
from Friend f join Highschooler h on (f.ID1 = h.ID)
where (f.ID1 in (select ID from Highschooler where name = 'Gabriel')
   or f.ID2 in (select ID from Highschooler where name = 'Gabriel'))
  and h.name != 'Gabriel'
```

#### Q2: For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like.
```SQL
SELECT y.name, y.grade, z.name, z.grade
FROM Likes x JOIN Highschooler y ON (x.ID1 = y.ID)
             JOIN Highschooler z ON (x.ID2 = z.ID)
WHERE y.grade >= z.grade+2
```

#### Q3: For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order.
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

#### Q4: Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade. 
```SQL
SELECT h.name, h.grade
FROM Highschooler h
WHERE h.ID NOT IN (SELECT Likes.ID1 FROM Likes)
   AND h.ID NOT IN (SELECT Likes.ID2 FROM Likes)
```
alternative answer (use `union` to combine the ID1 and ID2 of the Likes table and then rule out):
```SQL
select Highschooler.name, grade
From Highschooler left join 
(select ID1 as ID from Likes  union  select ID2 as ID from Likes) as joint on (Highschooler.ID = joint.ID)
where joint.ID is NULL
```

#### Q5: For every situation where student A likes student B, but we have no information about whom B likes (that is, B does not appear as an ID1 in the Likes table), return A and B's names and grades. 
```SQL
SELECT h1.name, h1.grade, h2.name, h2.grade
FROM Likes l JOIN Highschooler h1 ON (l.ID1 = h1.ID)
             JOIN Highschooler h2 ON (l.ID2 = h2.ID)
WHERE l.ID2 NOT IN 
(SELECT Likes.ID1
 FROM Likes)
```

#### Q6: Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade.
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

#### Q7: For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C. 
```SQL
SELECT x.name, x.grade, y.name, y.grade, z.name, z.grade
FROM (
      SELECT t.ID1, t.ID2, b.ID2 as ID3
      FROM (SELECT x.ID1, x.ID2 FROM Likes x WHERE x.ID2 NOT IN (SELECT y.ID2 FROM Friend y WHERE x.ID1 = y.ID1)) as t 
            JOIN Friend a ON (t.ID1 = a.ID1)
            JOIN Friend b ON (t.ID2 = b.ID1)
      WHERE a.ID2 = b.ID2
      ) as k 
      JOIN Highschooler x ON (k.ID1 = x.ID)
      JOIN Highschooler y ON (k.ID2 = y.ID)
      JOIN HIghschooler z ON (k.ID3 = z.ID)
```
Note the subquery "SELECT t.ID1, t.ID2, b.ID2 as ID3 FROM ... WHERE a.ID2=b.ID2)" gives (t.ID1, t.ID2) are Likes tuple but not in Friend. b.ID2 are common friends of t.ID1's friend and t.ID2's friend.


#### Q8: Find the difference between the number of students in the school and the number of different first names. 
```SQL
SELECT  total.num- COUNT(t.name)  
FROM (SELECT COUNT(h.name) as num
      FROM Highschooler h) as total, 
      (SELECT h.name
       FROM Highschooler h
       GROUP BY h.name) as t
```

#### Q9: Find the name and grade of all students who are liked by more than one other student. 
```SQL
SELECT h.name, h.grade
FROM Likes JOIN Highschooler h ON (Likes.ID2 = h.ID)
GROUP BY ID2
HAVING COUNT(ID1) >1
```
