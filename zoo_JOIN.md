# The JOIN operation


#### Q3: Modify it to show the player, teamid, stadium and mdate and for every German goal.
```SQL
select player, teamid, stadium, mdate
from game join goal on (game.id = goal.matchid)
where teamid = 'GER'
```
