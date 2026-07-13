# Intuition
When looking at this problem, I realized we need to find patients who have Type I Diabetes. The tricky part is that the disease code "DIAB1" can be mixed in with other diseases. It could be the very first disease in the list, or it could come after a space. So, we just need to search for that specific word using some text-matching rules.

# Approach
To solve this, I selected the required columns: `patient_id`, `patient_name`, and `conditions`. Then, I used the `WHERE` clause with the `LIKE` operator to filter the rows. 

I checked for two possibilities using an `OR` statement:
1. `conditions LIKE 'DIAB1%'`: This handles the case where "DIAB1" is the very first condition in the list.
2. `conditions LIKE '% DIAB1%'`: This handles the case where "DIAB1" is listed after another disease. The space before "DIAB1" is super important so we don't accidentally match a different disease that just happens to end with "DIAB1" (like "ABCDIAB1").

# Complexity
- Time complexity:
$$O(N)$$
Let $N$ be the number of rows in the `Patients` table. The database has to look through every single patient's conditions one by one to see if they match our text rules.

- Space complexity:
$$O(1)$$
We are just returning a filtered version of the data without creating any new or extra tables in memory, so the space we use stays constant.

# Code
```mysql []
# Write your MySQL query statement below
select
    patient_id,
    patient_name,
    conditions
from Patients 
where conditions LIKE '% DIAB1%' OR conditions LIKE 'DIAB1%';