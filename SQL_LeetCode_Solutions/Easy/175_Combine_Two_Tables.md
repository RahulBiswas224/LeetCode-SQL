# 175. Combine Two Tables

## Intuition

The core requirement is to retrieve a list of all people in the database, regardless of whether they have a registered address. Because a standard `INNER JOIN` strictly requires a match in both tables, it would filter out people without an address. Therefore, an outer join is required to preserve the base dataset.

## Approach

1. Use the `Person` table as the primary driving table (the "Left" table).

2. Execute a `LEFT JOIN` with the `Address` table.

3. Match the tables using the `ON` clause where `Person.personId = Address.personId`.

4. Select only the four specific columns requested by the prompt to minimize memory usage, rather than using `SELECT *`.

5. Because it is a `LEFT JOIN`, any `Person` without a matching `Address` record will automatically return `NULL` for the `city` and `state` columns, perfectly fulfilling the problem's edge-case requirement.

## Complexity

* Time complexity:
  $$O(N)$$
  *Where $N$ is the number of rows in the `Person` table. Because `personId` is the Primary Key (and therefore automatically indexed by the database engine), the lookup operation in the `Address` table executes in near $O(1)$ time per person.*

* Space complexity:
  $$O(N)$$
  *The memory required to hold the final output dataset scales linearly with the number of persons in the `Person` table.*

## Code

```mysql
SELECT 
    Person.firstName, 
    Person.lastName, 
    Address.city, 
    Address.state 
FROM Person
LEFT JOIN Address 
    ON Person.personId = Address.personId;
