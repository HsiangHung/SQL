# E. SQL Movie-Rating Modification Exercise

### Q1: Add the reviewer Roger Ebert to your database, with an rID of 209. 
```SQL
INSERT INTO Reviewer 
VALUES (209, 'Roger Ebert')
```

### Q2: Insert 5-star ratings by James Cameron for all movies in the database. Leave the review date as NULL. 
```SQL
INSERT INTO Rating (rID, mID, stars, ratingDate)  
SELECT 207, Movie.mID, 5, null
FROM Movie LEFT JOIN Rating ON (Rating.mID = Movie.mID)
GROUP BY Movie.mID
```
