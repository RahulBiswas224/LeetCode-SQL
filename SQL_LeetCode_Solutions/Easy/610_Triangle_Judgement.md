# Intuition
When I saw this problem, I immediately remembered my 9th and 10th grade math lessons! In geometry, we learned a rule called the "Triangle Inequality Theorem". It basically says that if you want to make a triangle, the sum of the lengths of any two sides must always be bigger than the length of the third side. 

# Approach
To solve this, I just need to translate that math rule into SQL. I can use the `IF` function to check the conditions. I need to check three things at the same time:
1. `x + y > z`
2. `x + z > y`
3. `y + z > x`

I will use `AND` to combine them. If all three are true, the database should say `'Yes'`. If even one of them is false, it means the lines can't close to form a triangle, so it should say `'No'`. I will name this new result column `triangle`.

# Complexity
- Time complexity:
$$O(N)$$
(Where N is the total number of rows in the `Triangle` table. The database simply reads each row one by one to do a quick math check, which takes constant time for each row.)

- Space complexity:
$$O(1)$$
(We don't need any extra memory or temporary tables to figure this out. We just print the result directly.)

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    x, y, z, 
    IF((x + y > z) AND (x + z > y) AND (y + z > x), 'Yes', 'No') AS triangle 
FROM Triangle;