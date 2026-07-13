# Intuition
When I first read this problem, I thought I just need to put all the students in groups based on their class. Once they are grouped, I just need to count them. If the count is 5 or more, that's the answer.

# Approach
First, I used `GROUP BY class` to group all the records by the class name. Then, to check the number of students in each group, I used the `HAVING` clause. I put `count(*) >= 5` to only keep the classes that have at least 5 students. Finally, I just `SELECT` the class name to show it in the output.

# Complexity
- Time complexity:
$$O(n)$$ 
The database has to check every row in the table to group them and count the students, where $n$ is the total number of rows.

- Space complexity:
$$O(n)$$ 
The database needs some extra memory to store the groups and their counts before giving the final answer.

# Code
```mysql []
# Write your MySQL query statement below
select 
    class
from Courses 
group by class
having count(*) >= 5;