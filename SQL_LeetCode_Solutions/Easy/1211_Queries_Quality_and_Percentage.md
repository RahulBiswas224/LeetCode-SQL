# Intuition
When I first read this problem, I realized we need to find the average scores for each type of query. Because we are looking at things "for each query," my first thought was that I definitely need to use `GROUP BY`. After that, it's just about applying the basic math formulas the question gives us for quality and percentage.

# Approach
1. First, I use `GROUP BY query_name` so that all the rows with the same query name are put together in one group.
2. I added `WHERE query_name IS NOT NULL` because we don't want to include any blank or missing query names in our final answer.
3. To find the **quality**, I calculate `rating / position` for each row, find the average using `AVG()`, and then use `ROUND(..., 2)` to keep only 2 decimal places.
4. To find the **poor_query_percentage**, I use an `IF` statement. `IF(rating < 3, 1, 0)` gives a 1 if it's a poor query, and 0 if it's not. I take the average of these 1s and 0s, multiply by 100 to make it a percentage, and round it to 2 decimal places.

# Complexity
- Time complexity:
$$O(N)$$
The database has to read through all $N$ rows in the `Queries` table one time to group them and do the math.

- Space complexity:
$$O(M)$$
Where $M$ is the number of unique `query_name`s. The database needs a little bit of extra memory to store the groups and the final calculated table.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    query_name,
    ROUND(AVG(rating / position), 2) AS quality,
    ROUND(AVG(IF(rating < 3, 1, 0)) * 100, 2) AS poor_query_percentage
FROM 
    Queries
WHERE 
    query_name IS NOT NULL
GROUP BY 
    query_name;