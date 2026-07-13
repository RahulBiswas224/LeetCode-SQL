# Intuition
The tricky part of this problem is that a person can be either the one who requested the friendship or the one who accepted it. To find out who has the most friends, I need to treat both `requester_id` and `accepter_id` as the same type of data: an "involved user." My first thought was to combine both columns into one big list of user IDs and then just count how many times each ID appears.

# Approach
My approach uses a Common Table Expression (CTE) to clean up the data before doing the heavy lifting:

1.  **Combine (CTE)**: I used `SELECT requester_id AS id` and `SELECT accepter_id AS id` combined with `UNION ALL`. Unlike a regular `UNION`, `UNION ALL` keeps every single record, which is exactly what we need to make sure we count every single friendship.
2.  **Grouping**: In the main query, I just `GROUP BY id` to put all of a person's friendship records into one group.
3.  **Counting & Sorting**: I used `COUNT(*)` to get the total number of friends for each person. Finally, I sorted the results by `num` in descending order (`DESC`) and used `LIMIT 1` to grab only the person with the highest number.



# Complexity
- Time complexity:
$$O(N \log N)$$
Let $N$ be the number of accepted requests. Using `UNION ALL` scans the table twice, and the `GROUP BY` and `ORDER BY` operations require sorting, which takes $O(N \log N)$ time.

- Space complexity:
$$O(N)$$
We are creating a new temporary table (`AllUsers`) that contains two entries for every single friendship record, so it grows linearly with the input.

# Code
```mysql []
# Write your MySQL query statement below
WITH AllUsers AS (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
)

SELECT 
    id, 
    COUNT(*) AS num
FROM AllUsers
GROUP BY id
ORDER BY num DESC
LIMIT 1;