# Intuition
When I read this problem, I thought about looking at my own posts on social media. The problem just wants us to find the authors who actually clicked on and read their own articles! So, if the person who wrote the article is the exact same person viewing it, that's our target.

# Approach
To solve this, I need to compare two columns in the `Views` table. 
1. First, I use a `WHERE` clause to check if `author_id = viewer_id`. If they are the same number, it means the author read their own work.
2. An author might read their own article a bunch of times, or read multiple articles they wrote. To make sure their ID only shows up once in our final list, I use the `DISTINCT` keyword.
3. The problem asks us to name the output column `id`, so I use `AS id` to rename it.
4. Finally, I use `ORDER BY author_id` to sort the IDs from smallest to biggest, just like the instructions asked.

# Complexity
- Time complexity:
O(N)
(Where N is the total number of rows in the `Views` table. The database has to look at every single view record to see if the author and viewer match, and then sort the final list.)

- Space complexity:
O(N)
(The database needs a little bit of temporary memory to hold the filtered list of authors, remove the duplicates, and sort them before showing the final result.)

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    DISTINCT author_id AS id
FROM Views 
WHERE author_id = viewer_id
ORDER BY author_id;