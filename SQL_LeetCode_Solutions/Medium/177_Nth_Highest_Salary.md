# Intuition
When I saw this problem, I needed to find the $N^{th}$ highest salary. If I want the 1st highest, I just pick the maximum. If I want the 2nd highest, I need to skip the first one and take the next. I realized I could use the `LIMIT` and `OFFSET` clauses, but I first needed to make sure all duplicate salaries were removed so that the rank is accurate.

# Approach
My approach was to use a function structure. 
1. **Adjustment**: The `LIMIT` and `OFFSET` clauses work by starting from 0. So, if someone asks for the 2nd highest salary, I actually need to skip 1 row (which is $N-1$).
2. **Distinct**: I used `SELECT DISTINCT salary` because if two people have the same highest salary, they should both be counted as the "1st" rank, and we want the unique values for our ranking.
3. **Ordering**: I used `ORDER BY salary DESC` to put the biggest salaries at the top.
4. **Pagination**: I used `LIMIT 1` to get only one result and `OFFSET N` (after updating $N$ to $N-1$) to skip the correct number of rows.

# Complexity
- Time complexity:
$$O(N \log N)$$
Because we are sorting the salaries to find the correct rank, and sorting takes $N \log N$ time.

- Space complexity:
$$O(N)$$
The database might need to create a temporary table or result set to sort the distinct salaries before it can pick the one at the requested offset.

# Code
```mysql []
# Write your MySQL query statement below
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N - 1;
  
  RETURN (
      SELECT DISTINCT salary
      FROM Employee
      ORDER BY salary DESC
      LIMIT 1 OFFSET N
  );
END