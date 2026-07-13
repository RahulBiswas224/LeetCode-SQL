# Intuition
For this problem, I need to find the names of managers who have at least 5 people reporting directly to them. I realized that the `Employee` table contains both the employees and the reference to their managers in one place. My first thought was to find the list of manager IDs that appear 5 or more times, and then use that list to get the actual names of those managers.

# Approach
1. First, I use a subquery (the inner `SELECT`) to look at the `Employee` table and count how many times each `managerId` appears. 
2. I use `GROUP BY managerId` to collect all the reports together for each boss.
3. I use `HAVING COUNT(*) >= 5` to filter this list so that I am only left with the managers who have 5 or more direct reports.
4. Finally, I use a `JOIN` to connect this list of "boss IDs" back to the main `Employee` table (specifically the `m.id` column). This allows me to pull the `m.name` for those specific IDs.



# Complexity
- Time complexity:
$$O(N)$$
Where $N$ is the number of rows in the `Employee` table. The database has to scan the table once to group the managers and then match them back to their names.

- Space complexity:
$$O(K)$$
Where $K$ is the number of managers who meet the criteria. The database needs a little bit of extra memory to store the list of manager IDs found in the subquery.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    m.name
FROM 
    Employee m
JOIN (
    SELECT managerId 
    FROM Employee 
    GROUP BY managerId 
    HAVING COUNT(*) >= 5
) AS reports ON m.id = reports.managerId;