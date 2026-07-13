# Intuition
The goal here is to find "power users" who meet two criteria: they have sent at least 3 prompts, and they have at least one specific prompt that used more tokens than their own personal average. Since I need to calculate averages and counts for each user, I knew I had to use `GROUP BY`. Because I have to filter based on these calculations (like the prompt count and the comparison between a max value and an average), I had to use `HAVING`.

# Approach
1. I start by grouping the data using `GROUP BY user_id` so that all the prompts sent by the same person are collected together.
2. I calculate the number of prompts they sent using `COUNT(prompt)` and their average token usage using `ROUND(AVG(tokens), 2)`.
3. To filter these groups, I use the `HAVING` clause:
    * `prompt_count >= 3` ensures we only look at active users.
    * `MAX(tokens) > AVG(tokens)` is the tricky part—it checks if the user's highest token-consuming prompt is actually higher than their average usage. If this is true, it means they have at least one outlier prompt that was "high usage" compared to their normal activity.
4. Finally, I use `ORDER BY avg_tokens DESC, user_id` to list the users with the highest average usage at the top. If two users have the same average, it sorts them by their `user_id` so the list is nice and orderly.

# Complexity
- Time complexity:
$$O(N \log N)$$
Where $N$ is the number of entries in the `prompts` table. The grouping and aggregation take linear time, but sorting the results at the end adds the $\log N$ factor.

- Space complexity:
$$O(K)$$
Where $K$ is the number of unique users. We need to store the intermediate aggregate values (count, average, and max) for every user while the database processes the `HAVING` condition and sorts the list.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    user_id, 
    COUNT(prompt) AS "prompt_count", 
    ROUND(AVG(tokens), 2) AS "avg_tokens" 
FROM 
    prompts 
GROUP BY 
    user_id 
HAVING 
    COUNT(prompt) >= 3 AND MAX(tokens) > AVG(tokens)
ORDER BY 
    avg_tokens DESC, user_id;