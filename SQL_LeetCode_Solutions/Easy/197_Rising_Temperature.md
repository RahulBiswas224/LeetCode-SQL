# Intuition
When I first saw this problem, I knew I had to compare temperatures from two different days. Since all the data is in just one table, my first thought was, "How do I compare a table to itself?" I realized I needed to put today's weather and yesterday's weather side by side, which means I have to use a Self Join!

# Approach
Here is how I solved it step by step:
1. I used a `JOIN` to connect the `Weather` table to itself. I gave them nicknames: `w1` (for today) and `w2` (for yesterday).
2. To make sure the dates match up correctly, I used `DATE_ADD`. This makes sure that `w1`'s date is exactly one day after `w2`'s date.
3. Next, I used a `WHERE` condition to check if today's temperature (`w1.temperature`) is bigger than yesterday's temperature (`w2.temperature`).
4. If it is bigger, I just `SELECT` the `id` of `w1` because that's what the problem wants!

# Complexity
- Time complexity:
  $$O(n)$$
  The database has to go through the rows to match the dates and compare the temperatures. (It might be faster if the database uses indexing, but basically it runs in $$O(n)$$ time).

- Space complexity:
  $$O(1)$$
  We are not creating any extra lists or huge memory tables, we are just filtering and returning the IDs.

# Code
```mysql []
# Write your MySQL query statement below
SELECT w1.id
FROM Weather w1
JOIN Weather w2 
  ON w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature > w2.temperature;