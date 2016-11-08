# 1. SELECT from world Tutorial

#### Q11: Show the name - but substitute Australasia for Oceania - for countries beginning with N.
```SQL
SELECT name,
       CASE WHEN continent= 'Oceania' THEN 'Australasia'
            ELSE continent END
  FROM world
 WHERE name LIKE 'N%'
```

#### Q12: Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B.
```SQL
SELECT name, 
    CASE WHEN continent IN ('Europe', 'Asia') THEN 'Eurasia'
            WHEN continent IN ('North America', 'South America', 'Caribbean') THEN 'America'
             ELSE continent END
 FROM world
WHERE name LIKE 'A%' OR name LIKE 'B%'
```


# 2. SELECT from nobel Tutorial

#### Q8: Show the Physics winners for 1980 together with the Chemistry winners for 1984.
```SQL
SELECT *
FROM nobel
WHERE subject = 'Physics' AND yr = 1980
         OR subject = 'Chemistry' AND yr = 1984
```

#### Q9: Show the winners for 1980 excluding the Chemistry and Medicine.
```SQL
select * 
from nobel
where yr = 1980 and subject not in ('Chemistry', 'Medicine')
```

#### Q13: List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```SQL
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE ('Sir%')
ORDER BY yr DESC, winner
```
