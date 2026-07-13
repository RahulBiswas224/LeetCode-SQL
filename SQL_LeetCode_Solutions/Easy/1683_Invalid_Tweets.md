# Intuition
When I looked at this problem, I saw that we just need to find the tweets that are too long. The rules say a tweet is invalid if its text has more than 15 characters. So, I just need a way to count the characters in each tweet and filter out the ones that are longer than 15.

# Approach
My approach is very straightforward. I use a `SELECT` statement to get the `tweet_id` from the `Tweets` table. 

To filter the rows, I use the `WHERE` clause. Inside the `WHERE` clause, I use the built-in `LENGTH()` function on the `content` column. This function counts how long the text is. If `LENGTH(content) > 15` is true, then the tweet is invalid, and the database will include its ID in our final answer.

# Complexity
- Time complexity:
$$O(N)$$
Let $N$ be the number of tweets in the table. The database has to go through every single row one by one to check the length of the content.

- Space complexity:
$$O(1)$$
We are just filtering the existing data and returning the IDs. We don't need to create any extra tables or use extra memory in the background.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    tweet_id
FROM 
    Tweets 
WHERE 
    LENGTH(content) > 15;