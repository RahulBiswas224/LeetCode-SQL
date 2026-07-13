# Intuition
When I first looked at this problem, I knew I had to figure out how many orders each customer made. After counting them up, I just needed to find a way to pick the customer who has the biggest number of orders.

# Approach
I decided to use a CTE (which stands for Common Table Expression). It's basically like making a temporary mini-table to help organize things before the final answer. 

Inside this temporary table called `CustomerCounts`, I grouped the data by `customer_number` so I could count their orders. Then, I used a cool function called `RANK()`. I told it to rank the customers based on their order count from highest to lowest using `DESC`. Finally, in my main query, I just selected the `customer_number` where their rank is exactly 1!

# Complexity
- Time complexity:
$$O(n \log n)$$
Because the database has to scan all the rows to group the orders, and then it has to sort them from highest to lowest to figure out the exact ranks. Sorting normally takes $$O(n \log n)$$ time.

- Space complexity:
$$O(n)$$
Because we are creating that temporary table in memory, which has to store the counts and the ranks for every single customer in the database.

# Code
```mysql []
# Write your MySQL query statement below
WITH CustomerCounts AS (
    SELECT 
        customer_number, 
        RANK() OVER (ORDER BY COUNT(*) DESC) as rnk
    FROM Orders
    GROUP BY customer_number
)
SELECT customer_number
FROM CustomerCounts
WHERE rnk = 1;