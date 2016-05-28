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





