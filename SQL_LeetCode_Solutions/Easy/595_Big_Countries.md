# Intuition
When I read the problem, my first thought was that we just need to find the countries that are very big. The question tells us a country is big if its area is 3 million or more, or if its population is 25 million or more. So, I just need to filter the table to find rows that match at least one of these two rules. 

# Approach
I will write a simple SQL query to solve this. I will use `SELECT` to pick only the `name`, `population`, and `area` columns from the `World` table. Then, I will use a `WHERE` clause to check our conditions. I am using the `OR` operator between the area rule (`area >= 3000000`) and the population rule (`population >= 25000000`) because only one of them needs to be true for the country to be included.

# Complexity
- Time complexity:
$$O(n)$$ because the database has to check every single row in the table one by one to see if it matches our conditions.

- Space complexity:
$$O(1)$$ because we are not using any extra memory to store new things, we are just outputting the results.

# Code
```mysql []
# Write your MySQL query statement below
select 
    name,
    population,
    area
from World 
where area >= 3000000 OR population >= 25000000;