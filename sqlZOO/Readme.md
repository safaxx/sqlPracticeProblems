[EXERCISE: SELECT WITHIN SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)

1. <b>Percentages of Germany</b></br>
```sql
   select name, 
   concat(cast(round(population*100/(select population from world where name='Germany'),0) as int),'%') as percentage
   from world 
   where continent='Europe' 
   ```
   
2. <b>Bigger than every country in Europe</b></br>
   ```sql
   select name 
   from world 
   where gdp > ALL(select gdp from world where continent='Europe' and gdp>0) 
   ```
   
 3. <b>Largest in each continent</b></br>
 ```sql
    with largest_cte as 
   (select *, rank() over(partition by continent order by area desc) rank
   from world )
   select continent, name, area
   from largest_cte
   where rank=1  
   ```
   
4. <b>Three time bigger</b></br>
```sql
   SELECT x.name, x.continent
   FROM world x
   WHERE x.population > ALL(SELECT population*3
                            FROM world y 
                            WHERE y.continent = x.continent</br>
                            AND x.name<>y.name) 
```
                            
                            
[EXERCISE: JOIN](https://sqlzoo.net/wiki/The_JOIN_operation)</br>

 1.<b>Show the name of all players who scored a goal against Germany.</b></br>
 ```sql
   SELECT DISTINCT(player)
   FROM game JOIN goal ON matchid = id 
   WHERE (team1='GER' OR team2='GER')
   and teamid != 'GER' 
   ```
 
 2. <b>Show teamname and the total number of goals scored.</b></br>
 ```sql
    SELECT teamname, COUNT(gtime)
    FROM eteam JOIN goal ON id=teamid
    GROUP BY teamname
    ```
    
3. <b>For every match involving 'POL', show the matchid, date and the number of goals scored.</b></br>
```sql
   SELECT matchid,mdate, count(*)
   FROM game JOIN goal ON matchid = id
   WHERE (team1 = 'POL' OR team2 = 'POL')
   GROUP BY matchid, mdate 
   ```
   
4. <b>For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'</b></br>
```sql
   SELECT matchid, mdate, count(gtime)
   from game join goal on id=teamid
   where teamid='GER'
   GROUP BY matchid, mdate, gtime
   ```

5. <b>List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.</b></br>
```sql
   SELECT mdate,
       team1,
       SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) AS score1,
       team2,
       SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) AS score2 FROM
    game LEFT JOIN goal ON (id = matchid)
    GROUP BY mdate,team1,team2
    ORDER BY mdate, team1, team2 
    ```


