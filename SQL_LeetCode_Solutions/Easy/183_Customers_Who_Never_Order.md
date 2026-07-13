# Intuition
When I first saw this problem, I thought it's just like finding out who didn't submit their homework in class. We have a list of all the customers, and a list of all the orders. We just need to find the people whose IDs are completely missing from the orders list.

# Approach
I used a cool SQL trick called `NOT EXISTS`. 
Here is how it works: I go through the `Customers` table one by one. For every customer, I check the `Orders` table to see if their ID is in there (`o.customerId = c.id`). 
The `NOT EXISTS` command tells the database: "Only show me this customer if you CANNOT find their ID in the orders table." 
I used `SELECT 1` inside the brackets because we don't actually care what they ordered, we just want to see if at least one order exists. If it doesn't, boom, they are our never-ordering customer!

# Complexity
- Time complexity:
$$O(N)$$
where N is the number of customers. The database basically has to check each customer to see if they made an order. (Usually, databases have indexes that make this super fast!)

- Space complexity:
$$O(1)$$
because we don't need to create any extra tables or use extra memory to solve it, we just return the names.

# Code
```mysql []
# Write your MySQL query statement below
SELECT c.name AS "Customers"
FROM Customers c
WHERE NOT EXISTS (
    SELECT 1 
    FROM Orders o 
    WHERE o.customerId = c.id
);
```