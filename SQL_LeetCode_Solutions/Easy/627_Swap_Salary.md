# Intuition
When I first saw this problem, I realized I couldn't just use a normal `SELECT` query. I actually need to change the data inside the table itself. I just need a way to flip the 'm' values to 'f', and the 'f' values to 'm' all at the same time.

# Approach
Since I need to modify existing data, I use the `UPDATE` command on the Salary table. Then, I use `SET` to change the values in the `sex` column. To do the flipping, the `IF` function is super helpful here. I tell it to check if the current sex is 'm'. If that's true, it changes it to 'f'. If it's false (meaning it's already 'f'), it changes it to 'm'.

# Complexity
- Time complexity:
$$O(n)$$
The database has to look at and update every single row in the table exactly once, where $n$ is the number of rows.

- Space complexity:
$$O(1)$$
We are just changing the data directly in the table, so we don't need to use any extra memory to store new tables or groups.

# Code
```mysql []
# Write your MySQL query statement below
UPDATE Salary
SET sex = IF(sex = 'm', 'f', 'm');