#### Table of Contents
  + NULL
  + COUNT
  + MIN / MAX
  + AVERAGE
  + GROUP BY
  + DISTINCT
  + HAVING
  + DATE functions
  + CASE statements

#### NULL
+ NULLs are different than a zero - they are cells where data does not exist.

+ When identifying NULLs in a WHERE clause, we write `IS NULL` or `IS NOT NULL`. We don't use `=`, because NULL is __not a value__ in SQL. Rather, it is a __property__ of the data.

+ There are two common ways in which you are likely to encounter NULLs:
  + NULLs frequently occur when performing a LEFT or RIGHT JOIN. You saw in the last lesson - when some rows in the left table of a left join are not matched with rows in the right table, those rows will contain some NULL values in the result set.
  + NULLs can also occur from simply missing data in our database.

#### COUNT statement
Aggregates the total count of (non NULL) rows from the table

Basic example:
```
SELECT COUNT(*) AS account_count  // the 'AS account_count' is optional but provides clarity
FROM accounts;
```

+ Use COUNT and a specific column of data to see if there are any NULL data within that column. It's rare to find a fully empty (NULL) row, so any individual column with less COUNT value than the total COUNT value means that the column has empty cells (e.g. missing data).

+ COUNT does not consider rows that have NULL values. Therefore, this can be useful for quickly identifying which rows have missing data. To grab the specific rows with the missing data (e.g. NULL)
```
SELECT *
FROM accounts
WHERE primary_poc IS NULL
```
+ COUNT Works for columns with text or numeric data

#### SUM statement
