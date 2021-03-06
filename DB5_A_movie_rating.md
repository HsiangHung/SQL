
## A. Moving-Rating Query Exercise


#### Q1: Find the titles of all movies directed by Steven Spielberg
```SQL
SELECT title 
FROM movie 
WHERE director = 'Steven Spielberg'
```

#### Q2: Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order.
```SQL
SELECT DISTINCT year
FROM Movie JOIN Rating ON (Movie.mID=Rating.mID)
WHERE stars > 3
ORDER BY year ASC
```

#### Q3: Find the titles of all movies that have no ratings. 
```SQL
SELECT title /*, stars */
FROM Movie LEFT JOIN Rating ON (Movie.mID = Rating.mID)
WHERE stars is NULL
```  
or
```SQL
SELECT m.title
FROM Movie m 
WHERE m.mID NOT IN (SELECT Rating.mID FROM Rating)
```

#### Q4: Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date. 
```SQL
SELECT Reviewer.name 
FROM Reviewer LEFT JOIN Rating ON (Reviewer.rID = Rating.rID)
WHERE Rating.ratingDate is NULL
```

#### Q5: Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars. 
```SQL
SELECT Reviewer.name, Movie.title, Rating.stars, Rating.ratingDate
FROM Rating JOIN Movie ON (Movie.mID = Rating.mID)
            JOIN Reviewer ON (Rating.rID = Reviewer.rID)
ORDER BY Reviewer.name, Movie.title, Rating.stars ASC 
```

#### Q6: For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie. 
```SQL
SELECT Reviewer.name, Movie.title/*, a.stars, a.ratingDate */
FROM Rating a JOIN Reviewer ON (a.rID = Reviewer.rID)
              JOIN Movie ON (a.mID = Movie.mID)
              JOIN Rating c ON (a.mID = c.mID AND a.rID = c.rID)
WHERE a.rID IN 
(SELECT rID FROM Rating b  WHERE a.mID = b.mID  GROUP BY rID  HAVING COUNT(rID) >1) 
AND a.ratingDate < c.ratingDate AND  a.stars < c.stars
```
or 
```SQL
select name, title
from (select mID, rID from rating group by mID, rID having count(rID) > 1) as t 
                           join rating r1 on (t.mID=r1.mID and r1.rID=t.rID)
                           join rating r2 on (t.mID=r2.mID and r2.rID=t.rID)
                           join reviewer on (t.rID= reviewer.rID)
                           join movie on (t.mID=movie.mID)
where r2.ratingDate > r1.ratingDate and r2.stars > r1.stars
```
note,  if without "AND a.ratingDate < c.ratingDate AND  a.stars < c.stars", the outcome is gives movives which are rated by the same reviewer twice.

**(More recently) an alternative and much easiler query**

Since each reviewer review the same movie at most twice, just simply do 
```SQL
select name, title
from rating x join rating y on (x.rID=y.rID and x.mID=y.mID and x.ratingDate < y.ratingDate)
              join movie on (x.mID = movie.mID)
              join Reviewer on (x.rID=reviewer.rID)
where x.stars < y.stars
```
if the problem is limited to only review the same moive`TWICE`, but there are 3 times or others, then we still need to prepare `(rID, mID)` table 
```SQL
select rID, mID
from rating
group by rID, mID
having count(*) = 2
```
and then join it to filter `(rID, mID)`
```SQL
select name, title
from rating x join rating y on (x.rID=y.rID)
              join (select rID, mID from rating group by rID, mID having count(*) =2) as z on (x.rID=z.rID)
              join movie on (x.mID = movie.mID)
              join Reviewer on (x.rID=reviewer.rID)
where x.mID=y.mID and x.mID=z.mID and x.ratingDate < y.ratingDate and x.stars < y.stars
```


#### Q7: For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title. 
```SQL
select title, MAX(stars)
from Movie join Rating on (Movie.mID = Rating.mID)
group by title
order by title
```
and more complicated query:
```SQL
SELECT DISTINCT x.title, Rating.stars
FROM Movie x JOIN Rating ON (x.mID = Rating.mID)
WHERE Rating.stars >=
   (SELECT MAX(Rating.stars)
    FROM Movie y JOIN Rating ON (y.mID = Rating.mID)
    WHERE x.title = y.title
   )
ORDER BY x.title
```
or 
```SQL
SELECT Movie.title, Rating.stars
FROM Movie JOIN Rating ON (Movie.mID = Rating.mID)
WHERE Movie.mID IN
  (SELECT r.mID
   FROM Rating r
   GROUP BY r.mID
   HAVING COUNT(r.rID) >= 1)
GROUP BY Movie.mID   
HAVING Rating.stars = MAX(Rating.stars)
ORDER BY Movie.title
```

#### Q8: For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title. 
```SQL
SELECT Movie.title, MAX(Rating.stars) - MIN(Rating.stars) as 'spread'
FROM Movie JOIN Rating ON (Movie.mID=Rating.mID)
GROUP BY Movie.title
ORDER BY spread DESC
```

#### Q9: Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.) 
```SQL
SELECT AVG(A.oldrating)-AVG(B.newrating)
FROM (SELECT AVG(Rating.stars) as oldrating
                  FROM Movie JOIN Rating ON (Rating.mID=Movie.mID) 
                  WHERE year < 1980
                  GROUP BY Movie.mID) as A,
      (SELECT AVG(Rating.stars) as newrating
                  FROM Movie JOIN Rating ON (Rating.mID = Movie.mID)
                  WHERE year > 1980
                  GROUP BY Movie.mID) as B
```










