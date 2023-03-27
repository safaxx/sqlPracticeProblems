<b>Questions  practiced on DataLemur!</b>

1. [Data Science Skills](https://datalemur.com/questions/matching-skills)
 ```sql
SELECT DISTINCT(candidate_id) FROM candidates
where skill in ('Python' ,'Tableau','PostgreSQL')
GROUP BY candidate_id
HAVING COUNT(skill) >=3
ORDER BY candidate_id;
```
2. [Page With No Likes](https://datalemur.com/questions/sql-page-with-no-likes)
```sql
SELECT pages.page_id
FROM pages LEFT JOIN page_likes 
on pages.page_id=page_likes.page_id
WHERE liked_date IS NULL
ORDER BY page_id 
```
3.[Unfinished Parts](https://datalemur.com/questions/tesla-unfinished-parts)
```sql
SELECT DISTINCT(part)
FROM parts_assembly
WHERE assembly_step IS NOT NULL AND finish_date IS NULL
```
4.[Laptop vs Mobile Viewership](https://datalemur.com/questions/laptop-mobile-viewership)
```sql
select
sum(case when device_type='laptop' then 1 else 0 end) laptop_views,
sum(case when device_type in('phone','tablet') then 1 else 0 end) mobile_views
from viewership
```
5.[Duplicate Job Listings](https://datalemur.com/questions/duplicate-job-listings)
```sql
select COUNT(company_id) as duplicate from
(
SELECT company_id, title, description, COUNT(*)
FROM job_listings
group by company_id, title, description
having COUNT(*)>1) as x
```
6.[Average Post Hiatus](https://datalemur.com/questions/sql-average-post-hiatus-1)
```sql
SELECT user_id, 
DATE_PART('day',MAX(post_date)-MIN(post_date)) AS days
FROM posts
WHERE DATE_PART('year', post_date)='2021'
GROUP BY user_id
HAVING count(user_id) >=2;
```
7.[Team Power Users](https://datalemur.com/questions/teams-power-users)
```sql
SELECT sender_id, COUNT(sender_id) as sent
FROM messages
WHERE DATE_PART('month',sent_date)='8' 
and DATE_PART('year',sent_date)='2022'
GROUP BY sender_id
ORDER  BY sent desc
LIMIT 2
```
