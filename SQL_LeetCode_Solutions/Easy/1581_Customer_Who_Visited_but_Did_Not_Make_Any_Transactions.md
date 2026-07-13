# Intuition
For this problem, we need to find customers who came to the store but left without buying anything. If we stick our `Visits` list and `Transactions` list together, the visits where nothing was bought won't have any transaction details. Those empty spots are exactly what we are looking for!

# Approach
1. First, I use a `LEFT JOIN` to combine the `Visits` table with the `Transactions` table using the `visit_id`. A `LEFT JOIN` is perfect here because it keeps every single visit on our list, even if there is no matching transaction.
2. Next, I use the `WHERE` clause with `t.amount IS NULL`. This acts like a filter to keep only the visits where the transaction side is completely blank—meaning they didn't buy anything at all.
3. Then, I use `GROUP BY v.customer_id` to bundle all the non-buying visits for each specific customer together.
4. To find out how many times they visited without buying, I use `COUNT(v.customer_id)`.
5. Finally, I use `ORDER BY count_no_trans` to sort the list neatly from smallest to largest.

# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of rows. The database has to scan through the tables to join and filter them, but sorting the final list using `ORDER BY` takes a bit of extra computational time.

- Space complexity:
$$O(K)$$
Where $K$ is the number of unique customers who didn't make a purchase. The database needs a little extra memory to hold these groups while it counts and sorts them before giving us the final answer.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    v.customer_id,
    COUNT(v.customer_id) AS "count_no_trans"
FROM 
    Visits AS v
LEFT JOIN 
    Transactions t
    ON v.visit_id = t.visit_id
WHERE 
    t.amount IS NULL
GROUP BY 
    v.customer_id
ORDER BY 
    count_no_trans;