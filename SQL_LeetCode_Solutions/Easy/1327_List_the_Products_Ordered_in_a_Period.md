# Intuition
For this problem, we need to find products that were ordered 100 times or more during February 2020. Since the product names are in one table and the order details are in another, I knew right away that I needed to `JOIN` them together. After that, it's just about filtering the dates and doing some basic addition.

# Approach
1. First, I use a `JOIN` to connect the `Products` table and the `Orders` table using the `product_id`. This gives me all the information I need in one place.
2. I use the `WHERE` clause with `BETWEEN '2020-02-01' AND '2020-02-29'` to only look at orders that happened in February 2020.
3. Then, I use `GROUP BY p.product_id` to put all the orders for the same product into their own group. 
4. For each group, I add up all the units ordered using `SUM(o.unit)`.
5. Finally, I use `HAVING SUM(o.unit) >= 100` to throw away any products that didn't reach the 100-unit goal. We use `HAVING` instead of `WHERE` here because we are checking the result of a `SUM()`.

# Complexity
- Time complexity:
$$O(N + M)$$
Where $N$ is the number of products and $M$ is the number of orders. The database needs to look through both tables to join them, filter the dates, and calculate the totals.

- Space complexity:
$$O(K)$$
Where $K$ is the number of unique products ordered in February. The database uses a little extra memory to keep track of these groups while it adds up the units.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    p.product_name, 
    SUM(o.unit) AS unit 
FROM 
    Products p 
JOIN 
    Orders o ON p.product_id = o.product_id
WHERE 
    o.order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY 
    p.product_id
HAVING 
    SUM(o.unit) >= 100;