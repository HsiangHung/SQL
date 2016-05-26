
## A. Moving-Rating Query Exercise


### Q1: Find the titles of all movies directed by Steven Spielberg
```SQL
SELECT title 
FROM movie 
WHERE director = 'Steven Spielberg'
```

### Q2: Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order.
```SQL
SELECT DISTINCT year
FROM Movie JOIN Rating ON (Movie.mID=Rating.mID)
WHERE stars > 3
ORDER BY year ASC
```

### Q3: Find the titles of all movies that have no ratings. 
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

### Q4: Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date. 
```SQL
SELECT Reviewer.name 
FROM Reviewer LEFT JOIN Rating ON (Reviewer.rID = Rating.rID)
WHERE Rating.ratingDate is NULL
```

### Q5: Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars. 
```SQL
SELECT Reviewer.name, Movie.title, Rating.stars, Rating.ratingDate
FROM Rating JOIN Movie ON (Movie.mID = Rating.mID)
            JOIN Reviewer ON (Rating.rID = Reviewer.rID)
ORDER BY Reviewer.name, Movie.title, Rating.stars ASC 
```

### Q6: For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie. 
```SQL
SELECT DISTINCT Reviewer.name, Movie.title/*, a.mID, a.rID, a.stars */
FROM Rating a JOIN Rating b ON (a.rID = b.rID AND a.mID=b.mID)
            JOIN Movie ON (a.mID = Movie.mID)
            JOIN Reviewer ON (Reviewer.rID = a.rID)
WHERE a.stars < b.stars AND a.ratingDate < b.ratingDate
ORDER BY a.mID, a.rID
```

### Q7: For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title. 
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

### Q8: For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title. 
```SQL
SELECT Movie.title, MAX(Rating.stars) - MIN(Rating.stars) as 'spread'
FROM Movie JOIN Rating ON (Movie.mID=Rating.mID)
GROUP BY Movie.title
ORDER BY spread DESC
```

### Q9: Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.) 
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










