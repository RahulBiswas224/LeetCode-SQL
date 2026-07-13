# Intuition
When I looked at this problem, I saw that the `start` time and `end` time for each machine's process are in completely different rows. To figure out how long a process took, I need to subtract the start time from the end time. My first thought was that this would be much easier if I could put the start and end times side-by-side in the exact same row first.

# Approach
My approach uses a subquery (which is basically a query inside another query). 

First, I built the inner query to group the data by both `machine_id` and `process_id`. Inside this group, I used a `CASE` statement wrapped in a `MAX()` function. This trick finds the 'start' timestamp and puts it in its own column, and does the same for the 'end' timestamp. Now, every process has one neat row with both its start and end times!

Second, I built the outer query to look at that new temporary table. I grouped the data by just the `machine_id`. I subtracted the `start_time` from the `end_time` to get the total time for each process, used the `AVG()` function to find the average time for each machine, and wrapped it all in `ROUND(..., 3)` to keep exactly three decimal places. 

# Complexity
- Time complexity:
$$O(N)$$
Let $N$ be the total number of rows in the `Activity` table. The database needs to scan through all the rows to group them by machine and process, and then scan the grouped results to find the averages.

- Space complexity:
$$O(M)$$
Let $M$ be the number of unique machine and process combinations. The database needs to create a temporary table in memory to hold the results of the inner query before doing the final math.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    machine_id,
    ROUND(AVG(end_time - start_time), 3) AS processing_time
FROM (
    SELECT 
        machine_id,
        process_id,
        MAX(CASE WHEN activity_type = 'start' THEN timestamp END) AS start_time,
        MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end_time
    FROM Activity
    GROUP BY machine_id, process_id
) AS process_times
GROUP BY machine_id;