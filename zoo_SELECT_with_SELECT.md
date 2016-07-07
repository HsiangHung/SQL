# SELECT within SELECT Tutorial
#### Q1: List each country name where the population is larger than that of 'Russia'.
```SQL
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

#### Q2: Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```SQL
SELECT name 
FROM world 
WHERE continent = 'Europe'
AND  gdp/population >  
(SELECT gdp/population
FROM world
```

#### Q3: List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```SQL
SELECT name, continent
FROM world
WHERE continent IN 
(SELECT continent 
FROM world
WHERE name IN ('Argentina','Australia')
)
ORDER BY name
```

#### Q4: Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```SQL
SELECT name, population
FROM world
WHERE population > ALL
(SELECT population
FROM world
WHERE name IN ('Canada')
)
AND population < 
(SELECT population 
FROM world
WHERE name IN ('Poland'))
```

#### Q5: Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany. 
```SQL
SELECT name, CONCAT(ROUND(population*100/(SELECT population FROM world WHERE name = 'Germany')),'%')
FROM world
WHERE continent = 'Europe'
```

#### Q6: Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values) 
```SQL
SELECT name
FROM world
WHERE continent != 'Europe' AND gdp >= ALL
(SELECT gdp 
FROM world 
WHERE gdp >0 AND continent = 'Europe')
```

#### Q7: Find the largest country (by area) in each continent, show the continent, the name and the area:
```SQL
SELECT continent, name, area
FROM world x
WHERE area >= ALL
(SELECT area 
 FROM world y
 WHERE x.continent = y.continent AND area >0)
```
or
```SQL
SELECT continent, name, area
FROM world
WHERE area IN 
(SELECT MAX(area) FROM world GROUP BY continent)
```

#### Q8: List each continent and the name of the country that comes first alphabetically.
```SQL
SELECT continent, name
FROM world x
WHERE name <= ALL
(SELECT name
 FROM world y
 WHERE x.continent = y.continent
 ORDER BY name)
```

#### Q9: Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```SQL
SELECT name, continent, population
FROM world x
WHERE continent IN 
(SELECT continent
 FROM world
 WHERE  population >= ALL
(SELECT population
 FROM world y
 WHERE x.continent=y.continent AND population >0
 ORDER BY population DESC) AND population <= 25000000)
```
or (easier)
```SQL
SELECT name, continent, population
FROM world
WHERE continent IN 
( SELECT continent 
FROM world
GROUP BY continent
HAVING MAX(population) <= 25000000)
```

#### Q10: Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
```SQL
SELECT w.name, w.continent 
from world w 
where w.population > 3 * 
(select max(population) 
 from world w1 
 where w1.continent = w.continent and w1.name != w.name limit 1)
```
