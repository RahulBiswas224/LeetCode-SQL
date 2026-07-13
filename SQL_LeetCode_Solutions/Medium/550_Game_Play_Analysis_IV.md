# Intuition
The goal here is to find the "fraction" of players who logged in on the day immediately after their very first login day. I need to identify two things:
1. The total number of unique players.
2. The count of players who played on the day exactly one day after their first appearance.

I realized I can find the first login date for every player using `MIN(event_date)` and then check if that same player has a record for that date plus one day.

# Approach
My approach uses a subquery to filter the data:
1. **First Login Dates**: In the subquery, I group by `player_id` and find the `MIN(event_date)`. I use `DATE_ADD(..., INTERVAL 1 DAY)` to calculate the "next day" for each player.
2. **Filtering**: The main query uses an `IN` clause to select only the rows from the `Activity` table where the `(player_id, event_date)` matches the pair (player, first_login_day + 1).
3. **Calculation**: I count these specific players and divide that number by the total count of distinct `player_id`s from the whole `Activity` table. I use `ROUND(..., 2)` to get the fraction to two decimal places as requested.



# Complexity
- Time complexity:
$$O(N)$$
Let $N$ be the number of rows in the `Activity` table. The database needs to perform a full scan to find the minimum dates and then match them against the main table.

- Space complexity:
$$O(P)$$
Where $P$ is the number of unique players. We need to store the first login dates for all players to perform the comparison.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    ROUND(COUNT(player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM Activity
WHERE (player_id, event_date) IN (
    SELECT 
        player_id, 
        DATE_ADD(MIN(event_date), INTERVAL 1 DAY)
    FROM Activity
    GROUP BY player_id
);