# Intuition
When I read this problem, it sounded pretty straightforward! We just need to filter a list of movies based on two simple rules: the ID has to be an odd number, and the movie can't be "boring". After picking out those specific movies, we just need to rank them from best to worst based on their ratings.

# Approach
To solve this, I can use a `WHERE` clause with two conditions joined by an `AND`:
1. First, I use the modulo operator `%` to check for odd IDs. If `id % 2 != 0`, it means the ID is odd (because it leaves a remainder of 1 when divided by 2).
2. Second, I make sure the description isn't boring by writing `description != 'boring'`.
3. Finally, I need to sort the results. I noticed a tiny typo in your code where you wrote `ratingdesc`! I will fix it to `ORDER BY rating DESC` (with a space) so the database understands it needs to sort the ratings in descending order (highest score to lowest score).

# Complexity
- Time complexity:
$$O(N \log N)$$
(Where N is the number of movies. Checking each row to see if it's odd and not boring takes $$O(N)$$time, but sorting the final list by rating usually takes$$O(N \log N)$$ time for the database to process.)

- Space complexity:
$$O(1)$$
(We don't need any extra memory space behind the scenes to calculate this, we just output the filtered list directly.)

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    *
FROM Cinema
WHERE id % 2 != 0 AND description != 'boring' 
ORDER BY rating DESC;