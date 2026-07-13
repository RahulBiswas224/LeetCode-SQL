# Intuition
The goal of this problem is to rank scores from highest to lowest. If two people have the exact same score, they should get the same rank, and there shouldn't be any "gaps" in the ranking numbers (like skipping 2 if two people tie for 1st place). This is exactly what the `DENSE_RANK()` function in SQL is designed to do!

# Approach
1. I select the `score` column because that's what we want to display.
2. I use the window function `DENSE_RANK()`. Inside it, I use `OVER(ORDER BY score DESC)` to tell the database to look at all the scores, sort them from the highest (`DESC`) to the lowest, and assign a rank to each one.
3. Because I used `DENSE_RANK`, if two people have the same score, they get the same rank number, and the next person behind them gets the very next rank number (e.g., if two people are tied for rank 1, the next person is rank 2, not rank 3). 
4. Since the problem doesn't ask us to group or filter anything else, that's all we need!



# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of rows in the `Scores` table. The database needs to sort the scores to calculate the correct ranks, and sorting takes $O(N \log N)$ time.

- Space complexity:
$$O(N)$$
The database needs to store the results of the ranking calculation in memory for all the rows before it gives us the final list.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    score,
    DENSE_RANK() OVER(ORDER BY score DESC) AS 'rank'
FROM Scores;