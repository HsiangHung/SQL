# E. SQL Movie-Rating Modification Exercise

#### Q1: Add the reviewer Roger Ebert to your database, with an rID of 209. 
```SQL
INSERT INTO Reviewer 
VALUES (209, 'Roger Ebert')
```

#### Q2: Insert 5-star ratings by James Cameron for all movies in the database. Leave the review date as NULL. 
```SQL
INSERT INTO Rating (rID, mID, stars, ratingDate)
select b.rID, a.mID, 5, null
From (select Movie.mID 
      from Movie left join Rating on (Movie.mID = Rating.mID) 
      group by Movie.mID) as a
join  (select Reviewer.rID from Reviewer where name = 'James Cameron') as b
```

#### Q3: For all movies that have an average rating of 4 stars or higher, add 25 to the release year. (Update the existing tuples; don't insert new tuples.) 
```SQL
UPDATE Movie
SET year = year+25
WHERE mID IN 
(SELECT Movie.mID
 FROM Movie JOIN Rating ON (Movie.mID = Rating.mID)
 GROUP BY Rating.mID
 HAVING AVG(Rating.stars) >= 4)
```

#### Q4: Remove all ratings where the movie's year is before 1970 or after 2000, and the rating is fewer than 4 stars. 
```SQL
DELETE FROM Rating
WHERE mID IN 
(SELECT mID FROM Movie WHERE year < 1970 OR year >2000) AND stars <4
```
