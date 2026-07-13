# Intuition
When I read this problem, I saw that I need to gather all the products sold on the exact same date and put them together in one row. My first thought was that grouping by the date is the way to go. But I also needed a way to count the unique products and somehow combine their names into one single string.

# Approach
My approach is to use `GROUP BY sell_date` so everything is organized by the day it was sold. 

To find out how many items were sold, I used `COUNT(DISTINCT product)`. The `DISTINCT` keyword is super important here so we don't count the same item twice if it was sold multiple times on the same day. 

To put all the product names in one column, I found a really cool MySQL function called `GROUP_CONCAT`. Inside it, I again used `DISTINCT` to avoid duplicates, added `ORDER BY product ASC` to sort the names alphabetically like a dictionary, and used `SEPARATOR ','` to put commas between them. Finally, I sorted the whole table by the date like the question asked!

# Complexity
- Time complexity:
$$O(N \log N)$$
Let $N$ be the number of rows in the table. The database needs to sort the rows to group them by date, and it also has to sort the product names alphabetically inside the `GROUP_CONCAT` function, which takes a bit of extra processing time.

- Space complexity:
$$O(N)$$
The database needs memory to hold the grouped dates and to build the new, longer strings of combined product names before showing the final result table.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    sell_date, 
    COUNT(DISTINCT product) AS num_sold, 
    GROUP_CONCAT(DISTINCT product ORDER BY product ASC SEPARATOR ',') AS products
FROM 
    Activities
GROUP BY 
    sell_date
ORDER BY 
    sell_date;