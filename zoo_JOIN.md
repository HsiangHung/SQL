# The JOIN operation


#### Q3: Modify it to show the player, teamid, stadium and mdate and for every German goal.
```SQL
select player, teamid, stadium, mdate
from game join goal on (game.id = goal.matchid)
where teamid = 'GER'
```

#### Q4: Show the team1, team2 and player for every goal scored by a player called Mario.
```SQL
select team1, team2, player
from game join goal on (game.id = goal.matchid)
where player like 'Mario%'
```

#### Q5: Show player, teamid, coach, gtime for all goals scored in the first 10 minutes.
```SQL
select player, teamid, coach, gtime
from game join goal on (game.id = goal.matchid)
                  join eteam on (goal.teamid = eteam.id)
where gtime <= 10
```


#### Q7: List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'.
```SQL
SELECT player
FROM goal JOIN game ON (id=matchid)
WHERE stadium = 'National Stadium, Warsaw'
```


#### Q8: Instead show the name of all players who scored a goal against Germany.
```SQL
SELECT  DISTINCT player
FROM game JOIN goal ON (id = matchid)
WHERE (teamid != 'GER' AND (team1 = 'GER' OR team2 = 'GER'))
```

#### Q9: Show teamname and the total number of goals scored.
```SQL
SELECT teamname, COUNT(teamid)
FROM eteam JOIN goal ON (id=teamid)
GROUP BY teamname
```
