# SELECT from world Tutorial

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
