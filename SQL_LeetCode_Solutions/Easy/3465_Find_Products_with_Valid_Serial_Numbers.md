# Intuition
The goal here is to find products that have a "valid" serial number hidden inside their description. The problem defines a valid serial number as starting with "SN", followed by 4 numbers, a dash, and then another 4 numbers. Since we are looking for a specific pattern inside a block of text, Regular Expressions (Regex) are definitely the way to go.

# Approach
1. I select the `product_id`, `product_name`, and `description` from the `products` table.
2. I use `REGEXP_LIKE()` in the `WHERE` clause to scan the `description` column for our specific pattern.
3. Here is how the pattern `'\\bSN[0-9]{4}-[0-9]{4}\\b'` works:
    * `\\b`: This is a "word boundary." It makes sure the serial number is standing alone and not hidden inside a longer, invalid word.
    * `SN`: This forces the text to start with these exact two letters.
    * `[0-9]{4}`: This tells the computer to look for exactly 4 digits (0-9).
    * `-`: This ensures there is a dash between the two sets of numbers.
    * `[0-9]{4}`: This looks for another 4 digits.
    * `\\b`: Another word boundary at the end to make sure the serial number stops right there.
4. Finally, I use `ORDER BY product_id` to make sure the output list is sorted nicely from the smallest ID to the largest.

# Complexity
- Time complexity:
$$O(N)$$
Where $N$ is the number of rows in the `products` table. The database must check the `description` field of every single product to see if it matches the regex pattern.

- Space complexity:
$$O(1)$$
We aren't creating any new tables or storing large lists; we are just filtering the existing data, so the extra space used is constant.

# Code
```mysql []
# Write your MySQL query statement below
SELECT
    product_id,
    product_name,
    description
FROM
    products 
WHERE
    REGEXP_LIKE(description, '\\bSN[0-9]{4}-[0-9]{4}\\b', 'c')
ORDER BY 
    product_id;