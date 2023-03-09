# LeetCode-SQL-problems
Problems on Leetcode I solved to brush up my SQL! <br>
<b></b>
<b>[1. Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order/description/)</b><br>
```sql
select name as Customers
from customers c
where c.id not in (select customerid from orders)
```
<b>[2. Calculate Special Bonus](https://leetcode.com/problems/calculate-special-bonus/description/)</b><br>
```sql
select employee_id, 
case<br>
    when employee_id%2 != 0 and name not like 'M%' then salary
    else 0
    end as bonus
from employees order by employee_id
```

<b>[3. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/description/)</b><br>
```sql
with second_cte as (select salary, dense_rank() over(order by salary desc) rn 
from employee)
select max(case when rn=2 then salary else null end) as SecondHighestSalary 
from second_cte
```

<b>[4. Rank Scores](https://leetcode.com/problems/rank-scores/description/)</b><br>
```sql
select score, dense_rank() over (order by score desc) as rank from scores
```

<b>[5. Conecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/) </b><br>
```sql
with con_cte as (select lag(num) over(order by id) as prev, num as currentnum, 
lead(num) over (order by id) as next from logs) 
select distinct(currentnum) as ConsecutiveNums
from con_cte 
where prev = currentnum and currentnum = next
```
<b>[6. Employees Earning More Than Their Managers](https://leetcode.com/problems/employees-earning-more-than-their-managers/description/)</b><br>
```sql
Select a.NAME AS Employee<br>
From Employee AS a JOIN Employee AS b
ON a.ManagerId = b.Id
AND a.Salary > b.Salary
``` 

<b>[7.Department Highest Salary ](https://leetcode.com/problems/department-highest-salary/description/)</b><br>
```sql
with highest_cte as (select d.name as Department, e.name as  Employee, salary, 
rank() over (partition by departmentId order by salary desc) as rank 
from department d join employee e on d.id = e.departmentId) 
select Department,Employee, salary from highest_cte
where rank = 1
```
<b>[8. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/description/)</b><br>
```sql
with highest_cte as (select d.name as Department, e.name as  Employee, salary, 
dense_rank() over (partition by departmentId order by salary desc) as rank 
from department d join employee e on d.id = e.departmentId)<br>
select Department,Employee, salary from highest_cte
where rank <=3
```

<b>[9.Duplicate Emails](https://leetcode.com/problems/duplicate-emails/description/)</b><br>
```sql
select Email 
from Person
group by Email
having count(*) > 1
```

<b>[10. Customer Placing the Largest Number of Orders](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/description/)</b><br>
```sql
with cte as (select  top 1 customer_number, count(customer_number) as highest_order
from orders
group by customer_number order by highest_order  desc)
select customer_number from cte
```

