
176. Second Highest Salary

Intuition

While using a MAX() subquery works perfectly for the second highest salary, it becomes messy and unscalable if you are ever asked for the 3rd, 4th, or Nth highest salary. The most optimized and scalable approach is to sort the unique salaries in descending order and selectively pluck the exact rank you need using the database's pagination commands (LIMIT and OFFSET).

Approach

Use SELECT DISTINCT salary to ensure that if two employees tie for the highest salary, it is only counted as one rank.

Use ORDER BY salary DESC to arrange the salaries from highest to lowest.

Apply LIMIT 1 OFFSET 1 (which can also be written as LIMIT 1, 1). OFFSET 1 tells the database to skip the first row (the highest salary), and LIMIT 1 tells it to only return exactly one row after that (the second highest).

Handling the Edge Case: Wrap that entire logic inside an outer SELECT (...) AS SecondHighestSalary. In SQL, if a scalar subquery returns no rows (for example, if there is only one employee so the OFFSET finds nothing), wrapping it in a SELECT forces it to evaluate as NULL. This perfectly satisfies the problem's requirement.

Complexity

Time complexity:

$$O(N \log N) \text{ or } O(1)$$

If there is no index on the salary column, the database must sort the table, resulting in $O(N \log N)$ time. However, if the salary column is indexed (as is standard practice for heavily queried numeric columns), the engine completely skips the sorting phase and does an $O(1)$ index scan to instantly grab the second value in the B-Tree.

Space complexity:

$$O(1)$$

The database engine processes the sort and limits in place or using a bounded top-K heap, requiring constant extra space.

Code

SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
