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

#### MIN and MAX statements
`MIN` and `MAX` are aggregators that again ignore NULL values.

Functionally, MIN and MAX are similar to COUNT in that they __can be used on non-numerical columns__. Depending on the column type, MIN will return the lowest number, earliest date, or non-numerical value as early in the alphabet as possible. As you might suspect, MAX does the opposite—it returns the highest number, the latest date, or the non-numerical value closest alphabetically to “Z.”

#### AVG statement
`AVG` returns the mean of the data - that is the sum of all of the values in the column divided by the number of values in a column. This aggregate function again ignores the NULL values in both the numerator and the denominator.

If you want to count NULLs as zero, you will need to use SUM and COUNT. However, this is probably __not a good idea__ if the NULL values truly just represent unknown values for a cell.

One quick note that a median might be a more appropriate measure of center for this data, but finding the __median__ happens to be a pretty difficult thing to get using SQL alone — so difficult that finding a median is occasionally asked as an interview question.

+ Finding the `Median` value
```
SELECT *
FROM (SELECT total_amt_usd
      FROM orders
      ORDER BY total_amt_usd
      LIMIT 3457) AS Table1
ORDER BY total_amt_usd DESC
LIMIT 2;
```
Since there are 6912 orders - we want the average of the 3457 and 3456 order amounts when ordered. This is the average of 2483.16 and 2482.55. This gives the median of 2482.855. This obviously isn't an ideal way to compute. If we obtain new orders, we would have to change the limit. SQL didn't even calculate the median for us. __The above used a SUBQUERY, but you could use any method to find the two necessary values, and then you just need the average of them.__

#### GROUP BY statement
+ `GROUP BY` can be used to aggregate data within subsets of the data. For example, grouping for different accounts, different regions, or different sales representatives.

+ (Key Concept) __Any column in the SELECT statement that is not within an aggregator must be in the GROUP BY clause.__

+ The `GROUP BY` __always goes between__ `WHERE` and `ORDER BY`.

+ `ORDER BY` works like `SORT` in spreadsheet software.

+ SQL evaluates the aggregations __before__ the `LIMIT` clause. If you don’t group by any columns, you’ll get a 1-row result—no problem there. __If you group by a column with enough unique values that it exceeds the LIMIT number, the aggregates will be calculated, and then some rows will simply be omitted from the results.__

This is actually a nice way to do things because you know you’re going to get the correct aggregates. If SQL cuts the table down to 100 rows, then performed the aggregations, your results would be substantially different.

```
SELECT a.name, MIN(total_amt_usd) smallest_order
FROM accounts a
JOIN orders o
ON a.id = o.account_id
GROUP BY a.name
ORDER BY smallest_order;
```

```
SELECT r.name, COUNT(*) num_reps
FROM region r
JOIN sales_reps s
ON r.id = s.region_id
GROUP BY r.name
ORDER BY num_reps;
```

+ You can `GROUP BY` multiple columns at once; __this is often useful to aggregate across a number of different segments__.

+ The order of columns listed in the `ORDER BY` clasue __does__ make a difference. You are ordering the columns from left to right.

+ The order of column names in the `GROUP BY` clause __does not__ matter - the result will be the same regardless.

+ You can substitute numbers for column names in the `GROUP BY` clause. Its generally recommended to do this only when y ou're grouping many columns.

+ __(Key Concept - again)__ Any column that is not within an aggregation must show up in your GROUP BY statement. It should return an error but it may run and give erroneous results.

