# Intuition
When I saw this problem, I knew I needed to filter a list of emails to find only the "valid" ones based on specific rules. Since email formats can be tricky to check with simple `LIKE` statements, I remembered that MySQL has a powerful tool called `REGEXP` (Regular Expressions) which is perfect for matching patterns in text.

# Approach
My approach is to use a regular expression inside the `WHERE` clause to define exactly what a "valid" email looks like.

Here is what the pattern `'^[A-Za-z0-9_]+@[A-Za-z]+\\.com$'` actually means:
- `^`: Starts the check at the very beginning of the email.
- `[A-Za-z0-9_]+`: The name part can have letters, numbers, or underscores (at least one character long).
- `@`: There must be an "@" symbol.
- `[A-Za-z]+`: The domain name part must be letters only.
- `\\.com`: The email must end exactly with ".com". The double slash `\\` is used because a single dot is a special character in regex, and we want to match a literal dot.
- `$`: Ensures the email ends right after ".com" and doesn't have extra characters.

Finally, I just `ORDER BY` the `user_id` to keep things organized as requested.

# Complexity
- Time complexity:
$$O(N)$$
Let $N$ be the number of rows in the `Users` table. The database has to look at each email one by one and compare it against the pattern.

- Space complexity:
$$O(1)$$
We are just returning the filtered results without creating any extra tables, so the extra space used is constant.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    user_id, 
    email
FROM 
    Users
WHERE 
    email REGEXP '^[A-Za-z0-9_]+@[A-Za-z]+\\.com$'
ORDER BY 
    user_id ASC;