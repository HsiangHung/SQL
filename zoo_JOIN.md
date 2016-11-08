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
