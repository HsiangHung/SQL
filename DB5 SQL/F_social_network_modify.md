# F. SQL Social-Network Modification Exercise

#### Q1: It's time for the seniors to graduate. Remove all 12th graders from Highschooler.
```SQL
DELETE FROM Highschooler
WHERE grade =12
```

#### Q2: If two students A and B are friends, and A likes B but not vice-versa, remove the Likes tuple.
```SQL
DELETE FROM Likes
WHERE ID1 IN (SELECT f.ID1 
       FROM Friend f, Likes l
       WHERE f.ID1 = l.ID1 
         AND f.ID2 = l.ID2
         AND l.ID1 NOT IN 
              (SELECT ID2 FROM Likes))
```

#### Q3: For all cases where A is friends with B, and B is friends with C, add a new friendship for the pair A and C. Do not add duplicate friendships, friendships that already exist, or friendships with oneself. (This one is a bit challenging; congratulations if you get it right.)
```SQL

```
