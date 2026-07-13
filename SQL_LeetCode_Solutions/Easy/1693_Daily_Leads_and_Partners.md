# Intuition
For this problem, we need to figure out the number of unique leads and unique partners for each car make on each specific day. Whenever a problem asks for totals "for each" something, my brain immediately jumps to `GROUP BY`. Since we are looking at unique people, I know I'll have to use `COUNT(DISTINCT ...)`.

# Approach
1. First, I select the `date_id` and `make_name` from the `DailySales` table because we want our final answer categorized by these two things.
2. I use `GROUP BY date_id, make_name` to bundle all the rows together. This means all the sales for a specific car brand on a specific day go into one single group.
3. To find the number of unique leads, I use `COUNT(DISTINCT lead_id)` and call it `unique_leads`. The `DISTINCT` keyword is super important here so we don't accidentally count the same person twice if they show up multiple times in one day.
4. I do the exact same thing for the partners by using `COUNT(DISTINCT partner_id)` and calling it `unique_partners`.

# Complexity
- Time complexity:
$$O(N)$$
Where $N$ is the total number of rows in the `DailySales` table. The database just has to scan through the whole table once to create the groups and count the unique IDs.

- Space complexity:
$$O(K)$$
Where $K$ is the number of unique combinations of dates and car makes. The database needs a little extra memory to hold these grouped results while it counts everything.

# Code
```mysql []
# Write your MySQL query statement below
SELECT
    date_id, 
    make_name, 
    COUNT(DISTINCT lead_id) AS "unique_leads", 
    COUNT(DISTINCT partner_id) AS "unique_partners"
FROM 
    DailySales 
GROUP BY 
    date_id, make_name;