# Intuition
When I first saw this problem, I knew I needed to find a number that appears at least three times in a row in the table. Since the table is ordered by `id`, I realized I could look at the current row's number and peek at the next two rows to see if they all match. A "Window Function" is perfect for this because it lets us look at upcoming rows without joining the table to itself.

# Approach
My approach is to use the `LEAD()` function, which is a really helpful tool for looking ahead at future rows:

1.  **Windowing**: I created a subquery that selects the `num` for the current row, the `num` for the very next row (`LEAD(num, 1)`), and the `num` for the row after that (`LEAD(num, 2)`).
2.  **Filtering**: In the outer query, I just compare these three values. If `num` matches `next_1` AND `num` matches `next_2`, it means we found three consecutive numbers!
3.  **Distinct**: The problem asks to only list each number once even if it appears in multiple streaks, so I used `DISTINCT` to remove duplicates.



# Complexity
- Time complexity:
$$O(N \log N)$$
The database needs to sort the `Logs` table by `id` to use the window function correctly, and sorting is the most time-consuming part here.

- Space complexity:
$$O(N)$$
The database creates a temporary result set in memory that stores the values for the current row and the two upcoming rows for every entry in the table.

# Code
```mysql []
# Write your MySQL query statement below
SELECT DISTINCT num AS ConsecutiveNums
FROM (
    SELECT num,
           LEAD(num, 1) OVER (ORDER BY id) AS next_1,
           LEAD(num, 2) OVER (ORDER BY id) AS next_2
    FROM Logs
) AS windowed_logs
WHERE num = next_1 AND num = next_2;