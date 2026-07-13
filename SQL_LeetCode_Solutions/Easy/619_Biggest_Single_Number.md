# Intuition
When I looked at this, I thought I just need to find the numbers that only show up one time. Then out of those, I just need to pick the biggest one.

# Approach
First, I group the numbers and use `HAVING count(*) = 1` so I only get the numbers that appear exactly once. Then, I sort them from biggest to smallest using `ORDER BY num DESC`. Since I only want the very biggest one, I use `LIMIT 1` at the end to just grab the top number. I also wrap the whole query inside another `SELECT` so that if there are no single numbers at all, it will show `null` instead of giving a completely blank result.

# Complexity
- Time complexity:
$$O(n \log n)$$ 
The database has to check all the rows to group and count them, where $n$ is the total number of rows. Then it has to sort those numbers to find the biggest one, which takes a little extra time.

- Space complexity:
$$O(n)$$ 
The database needs some extra memory to save the grouped numbers and their counts before sorting them.

# Code
```mysql []
# Write your MySQL query statement below
select (
    select num
    from MyNumbers
    group by num
    having count(*) = 1
    order by num desc
    limit 1
) as num;