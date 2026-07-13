# Intuition
When I read this problem, I thought about seasonal items in a store, like winter coats or Valentine's Day candy! We are basically acting like a store manager who wants to find which products were *only* sold during the first quarter of 2019 (January 1st to March 31st). If a product was sold even once outside of this specific time frame, we don't want it on our list.

# Approach
To figure this out, I can look at the earliest and latest dates a product was sold. 
1. First, I use a `JOIN` to connect the `Product` table (our catalog) with the `Sales` table (our receipts) using the `product_id`.
2. Next, I gather all the sales for each item using `GROUP BY p.product_id, p.product_name`. 
3. Now for the clever part! Instead of checking every single receipt one by one, I just check the extremes. I use the `HAVING` clause to make sure the *very first time* it was sold (`MIN(s.sale_date)`) is on or after January 1st, 2019. 
4. Then, I make sure the *very last time* it was sold (`MAX(s.sale_date)`) is on or before March 31st, 2019. If both the earliest and latest sales are inside this window, it means *all* the sales happened in the first quarter!

# Complexity
- Time complexity:
$$O(N)$$
(Where N is the total number of sales records. The database has to scan through all the receipts, group them by product, and find the minimum and maximum dates for each group.)

- Space complexity:
$$O(M)$$
(Where M is the number of unique products. The database needs a little bit of temporary memory to hold the grouped items and keep track of the max/min dates before giving us the final list.)

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    p.product_id,
    p.product_name
FROM Product AS p
JOIN Sales AS s
    ON p.product_id = s.product_id
GROUP BY p.product_id, p.product_name
HAVING MIN(s.sale_date) >= '2019-01-01' 
   AND MAX(s.sale_date) <= '2019-03-31';