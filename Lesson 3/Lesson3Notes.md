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
Adds up all the values from a single column. Unlike COUNT, you can only use SUM on numeric columns. However, SUM will ignore NULL values.

An important thing to remember: __aggregators only aggregate vertically - the values of a column__.

Examples:
```
// finds the total for each individual order that was spent on 'standard' and 'gloss' paper

SELECT standard_amt_usd + gloss_amt_usd AS total_standard_gloss
FROM orders;
```
-- and --
```
// finds the price per unit for a given paper type, but across all orders. In other words, though the 'price/standard_qty' paper varies from one order to the next. I would like this ratio across all of the sales made in the orders table.

SELECT SUM(standard_amt_usd)/SUM(standard_qty) AS standard_price_per_unit
FROM orders;
```

#### Calculation across rows (Simple Arithmetic)
  + source: https://mode.com/sql-tutorial/sql-operators/#arithmetic-in-sql

However, in SQL you can only perform arithmetic across columns on values in a given row. To clarify, you can only add values in multiple columns __from the same row__ together using +—if you want to add values across multiple rows, you'll need to use __aggregate functions__.

```
SELECT year,
       month,
       west,
       south,
       west + south AS south_plus_west
  FROM tutorial.us_housing_units
  ```

  -- or --
```
SELECT year,
       month,
       west,
       south,
       west + south - 4 * year AS nonsense_column
  FROM tutorial.us_housing_units
```
Use parentheses to manage the order of operations. For example, if you wanted to average the west and south columns:
```
SELECT year,
       month,
       west,
       south,
       (west + south)/2 AS south_west_avg
  FROM tutorial.us_housing_units
  ```