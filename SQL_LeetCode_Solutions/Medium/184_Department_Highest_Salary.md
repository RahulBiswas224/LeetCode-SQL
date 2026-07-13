# Intuition
For this problem, we need to find the person with the highest salary in every department. Since we have to look at each department separately to compare salaries, I thought about using a "window function." These are special tools in SQL that let us rank things within groups. By ranking salaries from highest to lowest inside each department, the people with the "highest salary" will always end up with a rank of 1.

# Approach
1. I used a `WITH` clause to create a temporary table called `RankedEmployees`. This keeps my code clean and easy to read.
2. Inside that temporary table, I used `JOIN` to connect the `Employee` table with the `Department` table so I could get the names of the departments.
3. I used the `RANK()` function to give each employee a number. The `PARTITION BY e.departmentId` part tells the database to reset the ranking for every new department. `ORDER BY e.salary DESC` makes sure the highest salary gets the number 1.
4. After creating that temporary table, the final step was simple: I just selected the columns I needed and added a `WHERE rnk = 1` filter to only keep the top earners.



# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of employees. The database has to join the tables and sort the employees within their departments to calculate the ranks, which takes $O(N \log N)$ time.

- Space complexity:
$$O(N)$$
The database needs to store the temporary table with all the ranked rows in memory before filtering them down to the winners.

# Code
```mysql []
# Write your MySQL query statement below
WITH RankedEmployees AS (
    SELECT 
        d.name AS Department,
        e.name AS Employee,
        e.salary AS Salary,
        RANK() OVER (PARTITION BY e.departmentId ORDER BY e.salary DESC) AS rnk
    FROM Employee e
    JOIN Department d ON e.departmentId = d.id
)
SELECT 
    Department, 
    Employee, 
    Salary
FROM RankedEmployees
WHERE rnk = 1;