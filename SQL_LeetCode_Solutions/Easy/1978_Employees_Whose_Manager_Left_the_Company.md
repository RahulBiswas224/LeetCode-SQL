# Intuition
For this problem, we need to find employees who make less than 30,000 and whose boss has left the company. If a manager leaves, their ID disappears from the employee list. So, my first thought was to match each employee with their manager. If we look for the manager's ID in the table and can't find it, that means they left!

# Approach
1. First, I use a `LEFT JOIN` on the exact same `Employees` table. This is called a self-join. I pretend there are two tables: one for the employees (I call it `e`) and one for the managers (I call it `m`).
2. I connect them by matching the employee's `manager_id` to the manager's `employee_id`. A `LEFT JOIN` is super important here because it keeps our employee on the list even if their manager is missing.
3. Next, I use the `WHERE` clause to add my filters. First, I check that the employee makes less than 30,000 (`e.salary < 30000`).
4. Then, I make sure the employee actually had a manager assigned to them in the first place (`e.manager_id IS NOT NULL`).
5. The real trick is checking `m.employee_id IS NULL`. This tells the database to only keep the rows where it tried to find the manager but failed because they are no longer in the company.
6. Finally, I sort the results by the employee's ID using `ORDER BY e.employee_id`.

# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of employees in the table. The database has to read through the table and do the joins, which takes some time, but sorting the final list of answers using `ORDER BY` is usually the heaviest part of the work.

- Space complexity:
$$O(N)$$
The database needs some extra temporary memory to hold the joined tables and to figure out the correct sorting order before giving us the final list.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    e.employee_id
FROM 
    Employees e
LEFT JOIN 
    Employees m ON e.manager_id = m.employee_id
WHERE 
    e.salary < 30000 
    AND e.manager_id IS NOT NULL 
    AND m.employee_id IS NULL
ORDER BY 
    e.employee_id;