<b> These are the solutions for the extra exercises on SqlZOO. You can visit the tutorial through the link!</b>

[EXERCISES: NSS Tutorial](https://sqlzoo.net/wiki/NSS_Tutorial)<br>
1. <b>Show the subject and total number of students who responded to 
question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.</b>
```sql
SELECT subject, SUM(response)
  FROM nss
 WHERE question='Q22'
   AND (subject='(8) Computer Science' or subject='(H) Creative Arts and Design')
GROUP BY subject
```

2.<b> Show the subject and total number of students who A_STRONGLY_AGREE to question 22 
for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.</b>
```sql
SELECT subject, SUM(A_STRONGLY_AGREE*RESPONSE/100)
  FROM nss
 WHERE question='Q22'
   AND (subject='(8) Computer Science' or subject ='(H) Creative Arts and Design')
GROUP BY subject
```

3. <b>Show the percentage of students who A_STRONGLY_AGREE to question 22 for the subject 
'(8) Computer Science' show the same figure for the subject '(H) Creative Arts and Design'.</b>

```sql
SELECT subject, sum(a_strongly_agree*response/100)*100/sum(response)
  FROM nss
 WHERE question='Q22'
   AND (subject='(8) Computer Science' or subject= '(H) Creative Arts and Design')
group by subject
```

4. <b>Show the average scores for question 'Q22' for each institution that include 'Manchester' in the name.</b>
```sql
SELECT institution, sum(score*response)/sum(response)
  FROM nss
 WHERE question='Q22'
   AND (institution LIKE '%Manchester%')
GROUP BY institution
```
5. <b>Show the institution, the total sample size and the number of computing students for institutions in Manchester for 'Q01'.</b>
```sql
SELECT institution,sum(sample), sum(case when subject ='(8) Computer Science' then sample else 0 end)
  FROM nss
WHERE question='Q01' 
AND (institution LIKE '%Manchester%') 
group by institution</b>
```
[Link to a better explaination of this question on stack overflow](https://stackoverflow.com/questions/64921991/looking-for-an-explantion-for-the-official-answer-to-sqlzoo-nss-tutorial-8)

[EXERCISE: WINDOW FUNCTIONS](https://sqlzoo.net/wiki/Window_functions)

1. <b>Show the party and RANK for constituency S14000024 in 2017. List the output by party</b>
```sql
SELECT party, votes,
       RANK() OVER (ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY party
```

2. <b>Use PARTITION to show the ranking of each party in S14000021 in each year.</b>
```sql
SELECT yr,party, votes,
      RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000021'
ORDER BY party,yr
```
3. <b>Use PARTITION BY constituency to show the ranking of each party in Edinburgh in 2017. Order your results so the winners are shown first, then ordered by constituency.</b>
```sql
select constituency, party, votes, 
rank() over(partition by constituency order by votes desc) posn
from ge
where constituency between 'S14000021' and 'S14000026'
and yr=2017
order by posn, constituency
```
4. <b>Show the parties that won for each Edinburgh constituency in 2017.</b>
```sql
select constituency, party 
from
(select constituency, party, rank() over(partition by constituency order by votes desc) posn
from ge
where constituency between 'S14000021' and 'S14000026'
and yr=2017
) x
where x.posn=1
order by posn 
```

5.<b>Show how many seats for each party in Scotland in 2017.</b>
```sql
SELECT t_posn.party, COUNT(*)
FROM
(SELECT party, 
RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) posn  
FROM ge
WHERE yr = 2017 AND constituency LIKE "S%"
 )t_posn
WHERE t_posn.posn = 1
GROUP BY t_posn.party
ORDER BY constituency, votes DESC
```



























