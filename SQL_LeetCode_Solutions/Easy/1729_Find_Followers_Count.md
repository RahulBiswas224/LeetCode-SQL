# Intuition
When I saw this problem, I realized I just need to find out how many followers every user has. Since the table lists every follower relationship, I figured I just need to bundle the rows by the user and count them up.

# Approach
To solve this, I used `GROUP BY user_id` to put all the records for each user into their own separate group. After that, I used `count(*)` to count how many rows (or followers) are inside each group, which gives us the total. I named this new column `followers_count`. Lastly, I added `ORDER BY user_id` because the question asks us to return the result sorted by the user's ID.

# Complexity
- Time complexity:
$$O(N \log N)$$
Let $N$ be the number of rows in the `Followers` table. The database needs some time to sort the data to group it together, and then it sorts it again for the final `ORDER BY` step.

- Space complexity:
$$O(U)$$
Let $U$ be the number of unique users. The database needs memory to store the grouped counts and return the final table.

# Code
```mysql []
# Write your MySQL query statement below
select
    user_id,
    count(*) as "followers_count" 
from Followers 
group by  user_id  
order by user_id;