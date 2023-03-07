[EXERCISE: SELECT WITHIN SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)

1. <b>Percentages of Germany</b></br>
   select name, </br>
   concat(cast(round(population*100/(select population from world where name='Germany'),0) as int),'%') as percentage</br>
   from world </br>
   where continent='Europe'
   
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
                            
                            
[EXERCISE: SUM AND COUNT](https://sqlzoo.net/wiki/SUM_and_COUNT)
