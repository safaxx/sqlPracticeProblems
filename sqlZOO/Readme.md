[SELECT WITHIN SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)\

1. select name, concat(cast(round(population*100/(select population from world where name='Germany'),0) as int),'%') as percentage
   from world 
   where continent='Europe'
