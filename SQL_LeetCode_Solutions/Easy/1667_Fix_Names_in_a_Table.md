# Intuition
For this problem, we need to fix people's names so that only the first letter is a capital letter and all the other letters are lowercase. It's basically like fixing bad typing! I knew right away that I would need to use SQL string functions to slice the words up, change their cases, and glue them back together.

# Approach
1. First, I need to get just the very first letter of the name. I use `SUBSTRING(name, 1, 1)` for this, and wrap it in `UPPER()` to make sure it's capitalized.
2. Next, I need the rest of the name. I use `SUBSTRING(name, 2)` to grab everything from the second letter all the way to the end. I wrap this part in `LOWER()` so that everything else is forced into lowercase.
3. Then, I use the `CONCAT()` function to glue the big first letter and the small remaining letters back into one single word.
4. Finally, the question asks us to return the result sorted by the user's ID, so I add `ORDER BY user_id` at the very end.

# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of users in the table. The database has to look at every single name to fix the string, which takes $O(N)$ time. However, sorting the final list using `ORDER BY` takes a bit more time, making the overall process $O(N \log N)$.

- Space complexity:
$$O(N)$$
The database needs some extra memory to hold our newly fixed strings and to sort the final table before displaying the results.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    user_id, 
    CONCAT(UPPER(SUBSTRING(name, 1, 1)), LOWER(SUBSTRING(name, 2))) AS name
FROM 
    Users
ORDER BY 
    user_id;