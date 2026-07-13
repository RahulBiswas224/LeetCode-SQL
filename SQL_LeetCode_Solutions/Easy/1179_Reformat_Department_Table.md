# Intuition
When I first looked at this problem, I saw that the months are given in rows, but the final answer wants each month to be its own column. My first thought was that I need to turn the rows into columns based on the department `id`. I figured I could use grouping and conditional checking to fix this.

# Approach
My approach is to group all the data by the department `id` using `GROUP BY`. Then, to make the new columns, I used the `IF` statement inside a `SUM` function. 

For each month column (like Jan_Revenue), the `IF` statement checks if the row's month is 'Jan'. If it is true, it takes the `revenue` amount. If it is false, it just puts `NULL`. I just copy-pasted and changed the month names for all 12 months. This easily pivots the table for us!

# Complexity
- Time complexity:
$$O(n)$$
Because the database only needs to go through the `Department` table once to group the rows and check the conditions.

- Space complexity:
$$O(1)$$
We are not using any extra tables or complex structures, just returning the final result table.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    id,
    SUM(IF(month = 'Jan', revenue, NULL)) AS Jan_Revenue,
    SUM(IF(month = 'Feb', revenue, NULL)) AS Feb_Revenue,
    SUM(IF(month = 'Mar', revenue, NULL)) AS Mar_Revenue,
    SUM(IF(month = 'Apr', revenue, NULL)) AS Apr_Revenue,
    SUM(IF(month = 'May', revenue, NULL)) AS May_Revenue,
    SUM(IF(month = 'Jun', revenue, NULL)) AS Jun_Revenue,
    SUM(IF(month = 'Jul', revenue, NULL)) AS Jul_Revenue,
    SUM(IF(month = 'Aug', revenue, NULL)) AS Aug_Revenue,
    SUM(IF(month = 'Sep', revenue, NULL)) AS Sep_Revenue,
    SUM(IF(month = 'Oct', revenue, NULL)) AS Oct_Revenue,
    SUM(IF(month = 'Nov', revenue, NULL)) AS Nov_Revenue,
    SUM(IF(month = 'Dec', revenue, NULL)) AS Dec_Revenue
FROM 
    Department
GROUP BY 
    id;