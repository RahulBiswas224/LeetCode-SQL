# Intuition
When I read this problem, it made me think of best friends who always do school projects together! We basically just need to find which actor and director are a "favorite duo" and have teamed up for at least 3 movies.

# Approach
To solve this, I need to look at the whole table and group the actor and director pairs together. 
1. First, I use `GROUP BY actor_id, director_id`. This gathers all the movies and puts them into little piles for every unique actor-director team.
2. Then, I need to count how many movies are in each pile. I can't use a normal `WHERE` clause for this because we are checking the totals *after* grouping them. Instead, I use the `HAVING` clause with `COUNT(*) >= 3`. 
3. Finally, I just `SELECT` the `actor_id` and `director_id` so the final answer only shows the ID numbers of the pairs who followed the rule.

# Complexity
- Time complexity:
$$O(N)$$
(Where N is the total number of rows in the `ActorDirector` table. The database has to scan through all the records to group them up and count them.)

- Space complexity:
$$O(N)$$
(The database needs some extra memory behind the scenes to store these grouped piles before filtering out the ones with less than 3 movies.)

# Code
```mysql []
# Write your MySQL query statement below
SELECT
    actor_id, 
    director_id
FROM ActorDirector 
GROUP BY actor_id, director_id
HAVING count(*) >= 3;