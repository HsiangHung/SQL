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
