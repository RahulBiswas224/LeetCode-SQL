# Intuition
For this problem, we need to find out the total distance each user has travelled. Because we want to include every single user—even the ones who haven't taken any rides at all—I immediately thought of using a `LEFT JOIN`. Then, it's just a matter of adding up the distances and sorting them exactly how the problem asked.

# Approach
1. First, I use a `LEFT JOIN` to combine the `Users` table with the `Rides` table using the user's ID. A `LEFT JOIN` is super important here because it makes sure users with zero rides don't disappear from our list.
2. I group everything by `u.id` so that we can calculate the total distance for each specific person. (It's better to group by ID rather than name, just in case two people share the same name).
3. To get the total distance, I use `SUM(r.distance)`. But, if a user has no rides, this sum will be blank (`NULL`). So, I wrap it in `IFNULL(..., 0)` to turn those blanks into a nice clean `0`.
4. Finally, I sort the list using `ORDER BY travelled_distance DESC` so the biggest travellers show up at the top. If two people travelled the exact same distance, I sort them alphabetically by adding `, name` to the order clause.

# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of users. The database has to look through all the rows to group them, but the heaviest part of the work is sorting the final list using `ORDER BY`.

- Space complexity:
$$O(N)$$
The database needs a bit of extra memory to hold all the grouped user totals in a temporary table while it sorts them before giving us the final answer.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    u.name,     
    IFNULL(SUM(r.distance), 0) AS "travelled_distance"
FROM 
    Users AS u 
LEFT JOIN 
    Rides AS r
    ON u.id = r.user_id
GROUP BY 
    u.id
ORDER BY 
    travelled_distance DESC, name;