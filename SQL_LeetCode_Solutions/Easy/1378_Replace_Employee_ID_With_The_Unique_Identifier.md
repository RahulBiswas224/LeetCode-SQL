# Intuition
When looking at this problem, I saw that we need to show the unique ID and the name of every employee. But the catch is that some employees do not have a unique ID yet. Even if they don't have one, their names still need to be in the final answer with a `NULL` for their ID. This immediately made me think of a Left Join.

# Approach
My approach is to use a `LEFT JOIN` starting with the `Employees` table as the main table. By doing this, we keep every single row from the `Employees` table. 

I joined the `Employees` table with the `EmployeeUNI` table using the `id` column, because that is the column they both share. If an employee has a matching `id` in the `EmployeeUNI` table, it pulls their `unique_id`. If they don't, the database automatically puts a `NULL` there, which perfectly solves the problem!

# Complexity
- Time complexity:
$$O(N)$$
Let $N$ be the number of rows in the `Employees` table. The database has to scan through the employees and match them with the unique IDs, which happens fairly quickly since we are just connecting two tables on a single key.

- Space complexity:
$$O(N)$$
The memory used depends on the number of employees, because the database has to store and return the final table with all $N$ employees.

# Code
```mysql []
# Write your MySQL query statement below
select
    eu.unique_id ,
    e.name  
from Employees as e
left join EmployeeUNI as eu
  on e.id = eu.id;