<b> These are the solutions for the extra tutorials on SqlZOO. You can visit the tutorial through the link!</b>

[NSS Tutorial](https://sqlzoo.net/wiki/NSS_Tutorial)<br>
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
