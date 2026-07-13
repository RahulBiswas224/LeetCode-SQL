# Intuition
When I first read this problem, I thought it was just about finding repeating emails. But the tricky part is we have to delete the extra ones and only keep the one with the smallest ID number. So, my first thought was: "If I compare two identical emails side-by-side, I should just throw away the one with the bigger ID."

# Approach
To do this, I did something my computer science teacher called a "Self Join". I took the `Person` table and pretended there are two of them, calling them `p1` and `p2`. 

Here is what I did step-by-step:
1. I matched `p1` and `p2` to see if they have the exact same email address (`p1.email = p2.email`).
2. Then, I checked their ID numbers. If `p1` has a bigger ID than `p2` (`p1.id > p2.id`), that means `p1` is the duplicate that was added later.
3. Finally, I used the `DELETE p1` command to remove that specific row. 

Because the computer checks every row this way, all the duplicate emails with bigger IDs get deleted, leaving only the very first one (the smallest ID) completely safe!

# Complexity
- Time complexity:
$$O(n^2)$$ 
Because we are joining the table with itself, the database basically has to check rows against other rows. (Note: sometimes databases optimize this to make it faster, but in the worst case, it's n-squared).

- Space complexity:
$$O(1)$$
We are deleting the rows directly from our original table. We don't need to make any extra tables or lists to store things, so we aren't using any extra memory!

# Code
```mysql []
# Write your MySQL query statement below
DELETE p1 
FROM Person p1, Person p2 
WHERE p1.email = p2.email AND p1.id > p2.id;