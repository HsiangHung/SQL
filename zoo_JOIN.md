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
