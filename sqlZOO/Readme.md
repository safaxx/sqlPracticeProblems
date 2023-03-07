[SELECT WITHIN SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)

1. <b>Percentages of Germany</b>
   select name, 
   concat(cast(round(population*100/(select population from world where name='Germany'),0) as int),'%') as percentage
   from world 
   where continent='Europe'
   
2. <b>Bigger than every country in Europe</b>
   select name 
   from world 
   where gdp > ALL(select gdp from world where continent='Europe' and gdp>0)
   
 3. <b>Largest in each continent</b>
    with largest_cte as(select *,
   rank() over(partition by continent order by area desc)rank
   from world )
   select continent, name, area
   from largest_cte
   where rank=1
