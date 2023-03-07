# LeetCode-SQL-problems
Problems on Leetcode I solved to brush up my SQL! <br>
<b></b>
<b>[1. Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order/description/)</b><br>
select name as Customers<br>
from customers c<br>
where c.id not in (select customerid from orders)<br>

<b>[2. Calculate Special Bonus](https://leetcode.com/problems/calculate-special-bonus/description/)</b><br>
select employee_id, <br>
case<br>
    when employee_id%2 != 0 and name not like 'M%' then salary<br>
    else 0<br>
    end as bonus<br>
from employees order by employee_id<br>

<b>[3. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/description/)</b><br>
with second_cte as (select salary, dense_rank() over(order by salary desc) rn <br>
from employee)<br>
select max(case when rn=2 then salary else null end) as SecondHighestSalary <br>
from second_cte<br>

<b>[4. Rank Scores](https://leetcode.com/problems/rank-scores/description/)</b><br>
select score, dense_rank() over (order by score desc) as rank from scores<br>

<b>[5. Conecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/) </b><br>
with con_cte as (select lag(num) over(order by id) as prev, num as currentnum, <br>
lead(num) over (order by id) as next from logs) <br>
select distinct(currentnum) as ConsecutiveNums<br>
from con_cte <br>
where prev = currentnum and currentnum = next<br>

<b>[6. Employees Earning More Than Their Managers](https://leetcode.com/problems/employees-earning-more-than-their-managers/description/)</b><br>
Select a.NAME AS Employee<br>
From Employee AS a JOIN Employee AS b<br>
ON a.ManagerId = b.Id<br>
AND a.Salary > b.Salary<br>

<b>[7.Department Highest Salary ](https://leetcode.com/problems/department-highest-salary/description/)</b><br>
with highest_cte as (select d.name as Department, e.name as  Employee, salary, <br> 
rank() over (partition by departmentId order by salary desc) as rank <br>
from department d join employee e on d.id = e.departmentId) <br>
select Department,Employee, salary from highest_cte<br>
where rank = 1<br>

<b>[8. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/description/)</b><br>
with highest_cte as (select d.name as Department, e.name as  Employee, salary, <br>
dense_rank() over (partition by departmentId order by salary desc) as rank <br>
from department d join employee e on d.id = e.departmentId)<br>
select Department,Employee, salary from highest_cte<br>
where rank <=3<br>

<b>[9.Duplicate Emails](https://leetcode.com/problems/duplicate-emails/description/)</b><br>
select Email <br>
from Person<br>
group by Email<br>
having count(*) > 1<br>

<b>[10. Customer Placing the Largest Number of Orders](https://leetcode.com/problems/customer-placing-the-largest-number-of-orders/description/)</b><br>
with cte as (select  top 1 customer_number, count(customer_number) as highest_order<br>
from orders<br>
group by customer_number order by highest_order  desc)<br>
select customer_number from cte<br>
  

