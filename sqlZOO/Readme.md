[EXERCISE: SELECT WITHIN SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)
'''sql
1. <b>Percentages of Germany</b></br>
   select name, </br>
   concat(cast(round(population*100/(select population from world where name='Germany'),0) as int),'%') as percentage</br>
   from world </br>
   where continent='Europe'
   '''
   
2. <b>Bigger than every country in Europe</b></br>
   select name </br>
   from world </br>
   where gdp > ALL(select gdp from world where continent='Europe' and gdp>0)
   
 3. <b>Largest in each continent</b></br>
    with largest_cte as </br>
   (select *, rank() over(partition by continent order by area desc) rank</br>
   from world )</br>
   select continent, name, area</br>
   from largest_cte</br>
   where rank=1
4. <b>Three time bigger</b></br>
   SELECT x.name, x.continent</br>
   FROM world x</br>
   WHERE x.population > ALL(SELECT population*3</br>
                            FROM world y </br>
                            WHERE y.continent = x.continent</br>
                            AND x.name<>y.name)
                            
                            
[EXERCISE: JOIN](https://sqlzoo.net/wiki/The_JOIN_operation)</br>

 1.<b>show the name of all players who scored a goal against Germany.</b></br>
   SELECT DISTINCT(player)</br>
   FROM game JOIN goal ON matchid = id </br>
   WHERE (team1='GER' OR team2='GER')</br>
   and teamid != 'GER'
 
 2. <b>Show teamname and the total number of goals scored.</b></br>
    SELECT teamname, COUNT(gtime)</br>
    FROM eteam JOIN goal ON id=teamid</br>
    GROUP BY teamname
    
3. <b>For every match involving 'POL', show the matchid, date and the number of goals scored.</b></br>
   SELECT matchid,mdate, count(*)</br>
   FROM game JOIN goal ON matchid = id </br>
   WHERE (team1 = 'POL' OR team2 = 'POL')</br>
   GROUP BY matchid, mdate
   
4. <b>For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'</b></br>
   SELECT matchid, mdate, count(gtime)</br>
   from game join goal on id=teamid</br>
   where teamid='GER'</br>
   GROUP BY matchid, mdate, gtime

5. <b>List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.</b></br>
   SELECT mdate,</br>
       team1,</br>
       SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) AS score1,</br>
       team2,</br>
       SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) AS score2 FROM</br>
    game LEFT JOIN goal ON (id = matchid)</br>
    GROUP BY mdate,team1,team2</br>
    ORDER BY mdate, team1, team2


