# Intuition
My first thought was that instead of directly looking for people who *didn't* sell to 'RED', it would be a lot easier to first find the people who *did* sell to 'RED'. Once I have that list of people, I can just pick everyone else from the sales team. 

# Approach
First, I wrote a smaller subquery inside the main query. In this subquery, I linked the `Company` and `Orders` tables together using `com_id`. I checked for the company name 'RED' to get the `sales_id` of everyone who sold to them. 

Then, in the main query, I looked at the `SalesPerson` table. I used the `NOT IN` command to filter out all the sales IDs that showed up in my subquery. Finally, I just selected the `name` column to show the names of the remaining salespeople.

# Complexity
- Time complexity:
The database has to look through the `Orders` and `Company` tables to find the matching IDs, and then check the `SalesPerson` table. So the time complexity is roughly $$O(n)$$, where $$n$$ is the total number of rows we are checking across the tables.

- Space complexity:
The database needs to store a temporary list of the `sales_id`s that match the 'RED' company in the subquery. So the space complexity is roughly $$O(m)$$, where $$m$$ is the number of sales made to the 'RED' company.

# Code
```mysql []
# Write your MySQL query statement below
select 
    s.name 
from  SalesPerson as s 
where s.sales_id NOT IN (
    select 
        o.sales_id
    from Company as c
    join Orders as o 
    on c.com_id = o.com_id
    where c.name = 'RED'
);