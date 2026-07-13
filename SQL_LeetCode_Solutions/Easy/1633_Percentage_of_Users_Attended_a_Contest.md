# Intuition
For this problem, we need to figure out what percentage of all users registered for each contest. To find a percentage, we need two pieces of information: how many people signed up for a specific contest, and how many users exist in total. 

# Approach
1. First, I use `GROUP BY contest_id` on the `Register` table so we can look at the sign-ups for one contest at a time.
2. To count how many users joined that specific contest, I use `COUNT(user_id)`.
3. To find the *total* number of users across the whole platform, I use a little mini-query inside the math formula: `(SELECT COUNT(user_id) FROM Users)`. 
4. I divide the contest users by the total users, multiply by 100.0 to turn it into a percentage, and then wrap it all in `ROUND(..., 2)` to keep just two decimal places.
5. Finally, the problem asks us to sort the results. I use `ORDER BY percentage DESC` to put the biggest percentages at the top. If two contests tie with the exact same percentage, I add `, contest_id ASC` so the smaller contest ID goes first.

# Complexity
- Time complexity:
$$O(N + M)$$
Where $N$ is the number of rows in the `Register` table and $M$ is the number of rows in the `Users` table. The database has to count the total users once, and then go through the registrations to group and count them. Sorting at the end takes a little extra time too.

- Space complexity:
$$O(K)$$
Where $K$ is the number of unique contests. The database just needs a bit of temporary memory to hold the groups and their percentage scores before it sorts and prints them out.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    contest_id, 
    ROUND(COUNT(user_id) * 100.0 / (SELECT COUNT(user_id) FROM Users), 2) AS percentage
FROM 
    Register
GROUP BY 
    contest_id
ORDER BY 
    percentage DESC, contest_id ASC;