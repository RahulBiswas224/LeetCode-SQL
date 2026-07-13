# Intuition
When I looked at this problem, I realized the data is split across two tables: one for the projects and one for the employees. To find the average experience for each project, I first need to connect the project to the actual employee data. Once they are connected, I can just group them by the project and calculate the average.

# Approach
First, I used a `LEFT JOIN` to link the `Project` table with the `Employee` table by matching the `employee_id` in both. Then, I used `GROUP BY project_id` so that the database looks at one project at a time. To get the average, I used the `AVG()` function on the `experience_years`. Finally, since we usually need to round averages in these problems, I wrapped the average in `ROUND(..., 2)` to keep exactly two decimal places.

# Complexity
- Time complexity:
$$O(N + M)$$
The database has to scan through the Project table (let's call its size $N$) and the Employee table (size $M$) to join them together and calculate the averages. 

- Space complexity:
$$O(P)$$
The database needs a little bit of extra memory to store the groups for each unique project (let's call the number of projects $P$) and calculate their final averages.

# Code
```mysql []
# Write your MySQL query statement below
select 
    p.project_id,
    round(avg(e.experience_years),2) as "average_years"  
from Project as p 
left join Employee as e
  on p.employee_id = e.employee_id 
group by project_id;