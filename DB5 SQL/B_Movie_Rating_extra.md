# B. Movie Rating Query Exercise Extra

### Q1: Find the names of all reviewers who rated Gone with the Wind. 
```SQL
SELECT DISTINCT Reviewer.name /*, Movie.title */
FROM Rating JOIN Reviewer ON (Rating.rID = reviewer.rID)
            JOIN Movie  ON (Rating.mID = Movie.mID)
WHERE Rating.mID = 101
```

### Q2: For any rating where the reviewer is the same as the director of the movie, return the reviewer name, movie title, and number of stars.
```SQL
SELECT Reviewer.name, Movie.title, Rating.stars
FROM Rating JOIN Reviewer ON (Rating.rID = Reviewer.rID)
            JOIN Movie ON (Rating.mID = Movie.mID)
WHERE Reviewer.name IN 
(SELECT Movie.director
 FROM Movie)
```

### Q3: Return all reviewer names and movie names together in a single list, alphabetized. (Sorting by the first name of the reviewer and first word in the title is fine; no need for special processing on last names or removing "The".)
```SQL
SELECT Reviewer.name FROM Reviewer
UNION
SELECT Movie.title FROM Movie
```

### Q4: Find the titles of all movies not reviewed by Chris Jackson.
```SQL
SELECT DISTINCT Movie.title
FROM Movie
WHERE Movie.mID NOT IN 
(SELECT Movie.mID 
 FROM Rating JOIN Reviewer ON (Rating.rID = Reviewer.rID)
             JOIN Movie ON (Movie.mID = Rating.mID)
 WHERE Reviewer.name IN ('Chris Jackson'))
```

### Q5: For all pairs of reviewers such that both reviewers gave a rating to the same movie, return the names of both reviewers. Eliminate duplicates, don't pair reviewers with themselves, and include each pair only once. For each pair, return the names in the pair in alphabetical order. 
```SQL
SELECT Reviewer.name, Movie.title, COUNT(Movie.mID)
FROM Rating JOIN Reviewer ON (Rating.rID = Reviewer.rID)
            JOIN Movie ON (Rating.mID = Movie.mID)
GROUP BY Movie.mID
HAVING COUNT(Movie.mID) >1
```
or 
```SQL
SELECT DISTINCT c.name, d.name
FROM Rating a JOIN Rating b 
              JOIN Reviewer c ON (a.rID = c.rID)
              JOIN Reviewer d ON (b.rID = d.rID)
WHERE a.mID = b.mID and a.rID != b.rID and c.name < d.name
```

### Q6: For each rating that is the lowest (fewest stars) currently in the database, return the reviewer name, movie title, and number of stars. 
```SQL
SELECT Reviewer.name, Movie.title, Rating.stars
FROM Rating JOIN Movie ON (Rating.mID = Movie.mID)
            JOIN Reviewer ON (Rating.rID = Reviewer.rID)
WHERE Rating.stars <= 
(SELECT Rating.stars
 FROM Rating)
```

### Q7: List movie titles and average ratings, from highest-rated to lowest-rated. If two or more movies have the same average rating, list them in alphabetical order. 
```SQL
SELECT Movie.title, AVG(Rating.stars)
FROM Rating JOIN Movie ON (Movie.mID = Rating.mID)
            JOIN Reviewer ON (Rating.rID = Reviewer.rID)
GROUP BY Movie.mID
ORDER BY AVG(Rating.stars) DESC, Movie.title
```

### Q8: Find the names of all reviewers who have contributed three or more ratings. (As an extra challenge, try writing the query without HAVING or without COUNT.)
```SQL
SELECT Reviewer.name /* , Movie.title   */
FROM  Rating JOIN Movie ON (Rating.mID = Movie.mID)
             JOIN Reviewer ON (Rating.rID = Reviewer.rID)
GROUP BY Reviewer.name
HAVING COUNT(Movie.title) >= 3
```

### Q9: Some directors directed more than one movie. For all such directors, return the titles of all movies directed by them, along with the director name. Sort by director name, then movie title. (As an extra challenge, try writing the query both with and without COUNT.) 
```SQL
SELECT Movie.title, Movie.director
FROM Movie
WHERE Movie.director IN 
  (SELECT Movie.director
   FROM Movie
   GROUP BY Movie.director
   HAVING COUNT(Movie.director) > 1 )
ORDER BY Movie.director, Movie.title
```
