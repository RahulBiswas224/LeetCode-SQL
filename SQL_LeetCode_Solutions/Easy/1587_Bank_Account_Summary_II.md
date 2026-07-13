# Intuition
When I read this problem, I understood that I need to calculate the total balance for each user. The balance is just the sum of all their transaction amounts. But I only want to show the people who have more than 10,000 in their account. Since the names are in one table and the money is in another, I need to connect them.

# Approach
My approach is to join the `Users` table with the `Transactions` table using the `account` number, because both tables share that column. I used a `LEFT JOIN` just in case a user exists but has no transactions (though an inner join would also work here).

Next, I grouped the data by the user's account using `GROUP BY`. This lets me gather all transactions for one person together. Then, I used the `SUM()` function to add up all their transaction amounts and called it "balance". 

Finally, since I only want users with a balance greater than 10,000, I used the `HAVING` clause to filter the grouped results. 

# Complexity
- Time complexity:
$$O(N + M)$$
Let $N$ be the number of users and $M$ be the number of transactions. The database has to look through all the transactions, join them with the users, and add the numbers up. 

- Space complexity:
$$O(U)$$
Where $U$ is the number of unique users who meet the condition (balance > 10000). The database just needs enough memory to store and return this final list of names and balances.

# Code
```mysql []
# Write your MySQL query statement below
select 
    u.name,
    sum(t.amount) as "balance"
from Users as u
left join Transactions as t 
  on u.account = t.account
group by u.account
having sum(t.amount) > 10000;