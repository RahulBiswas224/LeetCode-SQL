# Intuition
When I read the problem, my first thought was that I just need to find the very first date each player started playing. The "first date" basically means the smallest date value for each person. 

# Approach
To solve this, I put all the records for each player together using the `GROUP BY` clause on the `player_id`. Once everyone's logins are grouped up, I used the `MIN()` function on the `event_date` column. This picks out the earliest date they logged in. Finally, I renamed that result to `first_login` using `AS` so it matches what the output wants.

# Complexity
- Time complexity: O(n)
The database has to look through all the rows in the table once to group them and find the smallest date, so it takes linear time (where n is the number of rows).

- Space complexity: O(u)
The space depends on the number of unique players (u), because the final answer will only return one row for each player.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    player_id, 
    MIN(event_date) AS first_login
FROM 
    Activity
GROUP BY 
    player_id;