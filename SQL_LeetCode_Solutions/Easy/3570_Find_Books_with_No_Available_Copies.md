# Intuition
When I saw this problem, I realized we need to identify books where every single copy is currently out on loan. A book is "not available" if the number of people currently borrowing it matches the total number of copies the library owns. I knew I needed to compare the count of active loans (where the `return_date` is `NULL`) against the `total_copies` column from the books table.

# Approach
My approach was to join the `library_books` table with the `borrowing_records` table using the `book_id`. 

1.  **Filtering**: I used `WHERE b.return_date IS NULL` to only look at books that are currently being borrowed by someone.
2.  **Grouping**: I used `GROUP BY` on all the book details to collect the active loans for each specific book.
3.  **Comparing**: I used the `HAVING` clause to check if the `COUNT(b.record_id)` (how many records have no return date) is exactly equal to the `l.total_copies` column. This is the perfect way to find "sold out" or "checked out" books.
4.  **Sorting**: Finally, I used `ORDER BY` to list them as requested, with the number of borrowers first, and then alphabetically by title.

# Complexity
- Time complexity:
$$O(N \log N)$$
Let $N$ be the total number of borrowing records. Joining the tables and then grouping/sorting the result requires the database to process and order the data, which takes a bit of time.

- Space complexity:
$$O(N)$$
The database needs to hold the grouped results in memory to perform the `HAVING` count check before returning the final list.

# Code
```mysql []
# Write your MySQL query statement below
SELECT 
    l.book_id,
    l.title,
    l.author,        
    l.genre,    
    l.publication_year,
    l.total_copies AS current_borrowers
FROM 
    library_books l
JOIN 
    borrowing_records b ON l.book_id = b.book_id
WHERE 
    b.return_date IS NULL
GROUP BY 
    l.book_id, 
    l.title, 
    l.author, 
    l.genre, 
    l.publication_year, 
    l.total_copies
HAVING 
    COUNT(b.record_id) = l.total_copies
ORDER BY 
    current_borrowers DESC, 
    l.title ASC;