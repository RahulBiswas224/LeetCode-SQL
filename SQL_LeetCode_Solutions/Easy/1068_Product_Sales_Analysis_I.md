# Intuition
When I looked at this problem, I thought of it like having two different lists. One list is a bunch of sales receipts that only show a product's ID number, and the other list is a catalog that matches those IDs to the actual product names. To make sense of the sales, we just need to link the two lists together using that shared ID!

# Approach
To solve this, I can use a `LEFT JOIN` to combine the two tables. 
1. First, I start with the `Sales` table as my main list and give it a short nickname `s`. 
2. Then, I join it with the `Product` table (nicknamed `p`). 
3. I tell the database exactly how to connect them using `ON s.product_id = p.product_id`. This makes sure the correct name goes to the correct sale.
4. Finally, I just `SELECT` the specific columns we want to see in our final answer: the product's name, the year of the sale, and the price.

# Complexity
- Time complexity:
$$O(N)$$
(Where N is the number of rows in the `Sales` table. The database basically has to go through the sales records and look up the matching product name for each one.)

- Space complexity:
$$O(1)$$
(We don't need any extra memory space to store temporary data behind the scenes; we just connect the tables and print the combined results directly.)

# Code
```mysql []
# Write your MySQL query statement below
select 
    p.product_name,
    s.year,
    s.price
from Sales as s
left join Product as p
on s.product_id = p.product_id;