# Intuition
For this problem, we need to pick out users who have a perfectly valid email address based on some very strict rules. Because we are looking for a specific text pattern (like starting with a letter and ending with exactly "@leetcode.com"), I immediately thought of using Regular Expressions, or "Regex". Regex is basically a superpower tool in coding for finding patterns in text!

# Approach
1. I start by selecting the columns we need: `user_id`, `name`, and `mail` from the `Users` table.
2. I use the `WHERE` clause with `REGEXP_LIKE()` to filter the emails. This function checks if the email matches our exact pattern. 
3. Here is what the pattern `'^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$'` actually means:
    * `^` means we are looking at the very beginning of the text.
    * `[a-zA-Z]` means the very first character *must* be a letter (upper or lower case).
    * `[a-zA-Z0-9_.-]*` means the next characters can be letters, numbers, underscores, dots, or dashes. The `*` means there can be as many of these as we want, or none at all.
    * `@leetcode\\.com$` means the email *must* end exactly with "@leetcode.com". The `$` marks the very end of the text, making sure nothing sneaky is added after the ".com".
4. The `'c'` at the end of the function tells the database to be case-sensitive when checking the pattern.

# Complexity
- Time complexity:
$$O(N)$$
Where $N$ is the number of rows in the `Users` table. The database has to look at every single email one by one to check if it matches our pattern.

- Space complexity:
$$O(1)$$
We are just checking rows and not storing any extra information in memory while we do the matching, so the extra space used is constant.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    user_id, 
    name, 
    mail
FROM 
    Users
WHERE 
    REGEXP_LIKE(mail, '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$', 'c');