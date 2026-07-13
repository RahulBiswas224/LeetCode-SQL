# Intuition
When I read this problem, I realized we need to show every student paired with every single subject, even if they never took an exam for it. To get every possible combination of students and subjects, my first thought was to use a `CROSS JOIN`. After that, I just need to count how many times they actually showed up for the exam.

# Approach
First, I use a `CROSS JOIN` between the `Students` table and the `Subjects` table. This creates a list where every student is matched with every subject. 

Next, I use a `LEFT JOIN` to connect this list to the `Examinations` table. I match them using both the `student_id` and the `subject_name`. A `LEFT JOIN` is important here because if a student didn't take a test, they will still stay in our list (with blank or `NULL` exam data). 

Finally, I use `GROUP BY` to group the rows by student and subject. I use `COUNT(e.student_id)` to count how many exams they took. `COUNT` is great because it automatically ignores `NULL` values, giving us a neat `0` for exams they missed. Then I just sort the results like the question asked!

# Complexity
- Time complexity:
$$O(N \log N)$$
Let $N$ be the total number of combinations of students and subjects. The database has to join these tables and then sort (order) the final results, which takes some extra time.

- Space complexity:
$$O(N)$$
We need enough space in the memory to hold the final table with all the student and subject combinations before returning it.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    s.student_id, 
    s.student_name, 
    sub.subject_name, 
    COUNT(e.student_id) AS attended_exams
FROM 
    Students s
CROSS JOIN 
    Subjects sub
LEFT JOIN 
    Examinations e ON s.student_id = e.student_id AND sub.subject_name = e.subject_name
GROUP BY 
    s.student_id, 
    s.student_name, 
    sub.subject_name
ORDER BY 
    s.student_id, 
    sub.subject_name;