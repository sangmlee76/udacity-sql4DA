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

+ When identifying NULLs in a WHERE clause, we write IS NULL or IS NOT NULL. We don't use `=`, because NULL is __not a value__ in SQL. Rather, it is a __property__ of the data.

+ There are two common ways in which you are likely to encounter NULLs:
  + NULLs frequently occur when performing a LEFT or RIGHT JOIN. You saw in the last lesson - when some rows in the left table of a left join are not matched with rows in the right table, those rows will contain some NULL values in the result set.
  + NULLs can also occur from simply missing data in our database.

+