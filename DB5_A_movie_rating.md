
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
note,  if without "AND a.ratingDate < c.ratingDate AND  a.stars < c.stars", the outcome is gives movives which are rated by the same reviewer twice.

**(More recently) an alternative thought** is first to search which reviewer rated more than one movies.
First do
```SQL
select a.rID, count(a.mID)
from Rating a join Rating b on (a.rID = b.rID)
group by a.rID
```
which will gives 201: 4, 202:1, 203: 9... it operates as self join so double counting. Then next
```SQL
select a.rID, count(a.mID)
from Rating a join Rating b on (a.rID = b.rID)
where a.mID = b.mID and a.ratingDate < b.ratingDate and a.stars < b.stars
group by a.rID
```
gives 201:1. This removes double counting since `a.rating < b.ratingDate` and second ratings better than first rating gives `a.stars < b.stars`. So this query gives a list of reviews who rated the same movie twice and second rating is better than the first.
Then
```SQL
select Reviewer.name, Movie.title
from Rating join Movie on (Movie.mID = Rating.mID)
            join Reviewer on (Reviewer.rID = Rating.rID)
where Rating.rID in (
select a.rID
from Rating a join Rating b on (a.rID = b.rID)
where a.mID = b.mID and a.ratingDate < b.ratingDate and a.stars < b.stars
group by a.rID)
group by Rating.mID
having count(Rating.mID) >1
```
First we select reviewers (`Rating.rID`) from the subquery `select a.rID ... group by a.rID` and then group by `mID`.




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










