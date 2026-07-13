# Intuition
When I first looked at this problem, I knew I had to combine the employee list with their bonuses. But the catch is that some employees don't get a bonus at all. If I used a regular join, those people without bonuses would just disappear from the list. I needed to make sure I included people with no bonus at all, along with the people who got a bonus of less than 1000.

# Approach
I used a `LEFT JOIN` to connect the `Employee` table with the `Bonus` table. A `LEFT JOIN` is perfect here because it keeps every single employee from the first table, even if they don't have a matching bonus in the second table (it just fills their bonus in as `NULL`). After joining them, I added a `WHERE` clause to filter the final list. I told it to only keep rows where the bonus is strictly less than 1000 (`b.bonus < 1000`) OR where they didn't get a bonus at all (`b.bonus IS NULL`).

# Complexity
- Time complexity: $O(N)$
The database has to scan through the tables to join them together and check our conditions. If there are N employees, it generally takes time proportional to the number of people.

- Space complexity: $O(N)$
The memory used depends on how many rows match our conditions. In the worst case, if every single employee fits the criteria, it will take up space for N rows to show the final result.

# Code
```mysql []
# Write your MySQL query statement below
select
    e.name as "name",
    b.bonus as "bonus"
from Employee as e 
left join Bonus as b 
  on e.empId = b.empId
    where b.bonus is NULL OR b.bonus < 1000 ;