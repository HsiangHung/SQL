# B. Movie Rating Query Exercise Extra

### Q1: Find the names of all reviewers who rated Gone with the Wind. 
```SQL
SELECT DISTINCT Reviewer.name /*, Movie.title */
FROM Rating JOIN Reviewer ON (Rating.rID = reviewer.rID)
            JOIN Movie  ON (Rating.mID = Movie.mID)
WHERE Rating.mID = 101
```
