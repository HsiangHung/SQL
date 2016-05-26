
# This is the folder to have SQL solutions of Stanford DB5 courses.

## Moving-Rating Query Exercise

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










