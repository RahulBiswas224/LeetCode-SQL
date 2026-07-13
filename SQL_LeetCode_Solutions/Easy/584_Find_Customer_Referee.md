# Intuition
At first, I thought we just needed to find people who were not referred by the person with ID 2. So, my first idea was simply using `referee_id != 2`. But then I remembered from class that SQL acts weird with empty spaces, meaning it ignores `NULL` values when you do a standard "not equal to" check. So, I realized I also have to check for `NULL` separately!

# Approach
I decided to select the `name` column from the `Customer` table (which I nicknamed `c` just to make it shorter). Then, I used the `WHERE` clause to filter the rows. I put in two conditions and connected them with an `OR`: either the `referee_id` is not equal to 2, or the `referee_id` is completely empty (`IS NULL`). This makes sure nobody gets left out by accident.

# Complexity
- Time complexity:
$$O(n)$$
Because the database has to scan through all the rows in the customer table one by one to check my conditions.

- Space complexity:
$$O(1)$$
Because we are just reading and filtering the data, not creating any new large tables or storing extra stuff in memory.

# Code
```mysql []
# Write your MySQL query statement below
select 
    c.name
from Customer as c
where referee_id != 2 OR referee_id is NULL;