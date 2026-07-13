# Intuition
When I looked at this problem, I realized we need to find how many unique users were active on each day. But we don't want all the data—we only care about a specific 30-day window that ends on 2019-07-27. So, I need to filter out the wrong dates first, group them by each day, and then count the users.

# Approach
First, I used the `WHERE` clause with `BETWEEN` to only keep the dates that fall into the 30-day period from '2019-06-28' to '2019-07-27'. Next, I used `GROUP BY activity_date` so the database counts the users for each day separately. In the `SELECT` line, I used `COUNT(DISTINCT user_id)` because a single user might do multiple activities on the same day, but we only want to count them once as an active user for that day. Finally, I used `as` to rename the columns so they match the expected output.

# Complexity
- Time complexity:
$$O(n)$$
The database has to look through all the rows in the table to filter the dates and group them, where $n$ is the total number of rows.

- Space complexity:
$$O(k)$$
The database needs a small amount of extra memory to store the grouped rows for the final output, where $k$ is the number of unique days within that 30-day window (which is at most 30 days).

# Code
```mysql []
# Write your MySQL query statement below
select 
    activity_date as day, 
    count(distinct user_id) as active_users
from Activity
where activity_date between '2019-06-28' and '2019-07-27'
group by activity_date;