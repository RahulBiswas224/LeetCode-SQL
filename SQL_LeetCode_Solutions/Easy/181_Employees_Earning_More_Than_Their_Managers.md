## Intuition
We need to compare two rows within the same table. By performing a **self-join** linking the employee's `managerId` to the manager's `id`, we line up the employee and their manager side-by-side in a single row. From there, comparing their salaries is just a simple filter condition.

# Approach
1. Initialize a [data structure, e.g., hash map].
2. Iterate through the input array.
3. For each element, check if [condition].
4. If it is, return the result. Otherwise, add the current element to the [data structure].

# Complexity
- **Time complexity:** $O(n)$ - **Space complexity:** $O(n)$

# Code
```mysql []
SELECT 
    e.name AS Employee
FROM Employee e
JOIN Employee m 
    ON e.managerId = m.id
WHERE e.salary > m.salary;
```
