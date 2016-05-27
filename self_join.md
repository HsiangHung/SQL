# Self join
Q1: How many stops are in the database.
```SQL
SELECT COUNT(stops.name)
FROM stops
```

Q3: Give the id and the name for the stops on the '4' 'LRT' service.
```SQL
SELECT id, name
FROM stops JOIN route ON (stops.id = route.stop)
WHERE num = 4 AND company = 'LRT'
```
