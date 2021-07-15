**Source Note: Unless tagged with a `<PINS>`, all content (e.g. text, images, etc.) on this page is from the Udacity's SQL for Data Analysis course.  This page is being used as 'digital highlighting' for future reference and for personal use.**


### [How Databases Store Data](https://classroom.udacity.com/courses/ud198/lessons/614cf95a-13bf-406c-b092-e757178e633b/concepts/45d7a40b-6cd7-4171-88a3-434ac16055af)

#### A few key points about data stored in SQL databases:

+ Data in databases is stored in tables that can be thought of just like Excel spreadsheets.
For the most part, you can think of a database as a bunch of Excel spreadsheets. Each spreadsheet has rows and columns. Where each row holds data on a transaction, a person, a company, etc., while each column holds data pertaining to a particular aspect of one of the rows you care about like a name, location, a unique id, etc.

+ All the data in the same column must match in terms of data type.
An entire column is considered quantitative, discrete, or as some sort of string. This means if you have one row with a string in a particular column, the entire column might change to a text data type. This can be very bad if you want to do math with this column!

+ Consistent column types are one of the main reasons working with databases is fast.
Often databases hold a LOT of data. So, knowing that the columns are all of the same type of data means that obtaining data from a database can still be fast.

#### Article: [SQLite vs MySQL vs PostgreSQL: A Comparison Of Relational Database Management Systems](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems)

#### The key to SQL is understanding statements. A few statements include:

+ CREATE TABLE is a statement that creates a new table in a database.
+ DROP TABLE is a statement that removes a table in a database.
+ SELECT allows you to read data and display it. This is called a query.
  + The SELECT statement is the common statement used by analysts, and you will be learning all about them throughout this course!

#### SELECT and FROM in Every SQL Query
+ Every query will have at least a SELECT and FROM statement.
+ The SELECT statement is where you put the columns for which you would like to show the data.
+ The FROM statement is where you put the tables from which you would like to pull data.

```
SELECT *
FROM orders;
```

#### Formatting best practices
Even though SQL is case-insensitive, **it is common and best practice to capitalize all SQL commands, like SELECT and FROM, and keep everything else in your query lower case.**

Capitalizing command words makes queries easier to read, which will matter more as you write more complex queries. For now, it is just a good habit to start getting into, to make your SQL queries more readable.

One other note: The text data stored in SQL tables can be either upper or lower case, and SQL is case-sensitive in regard to this text data.

#### Avoid Spaces in Table and Variable Names
It is common to use underscores and avoid spaces in column names. It is a bit annoying to work with spaces in SQL. In Postgres if you have spaces in column or table names, you need to refer to these columns/tables with double quotes around them (Ex: FROM "Table Name" as opposed to FROM table_name). In other environments, you might see this as square brackets instead (Ex: FROM [Table Name]).

#### Semicolons
Depending on your SQL environment, your query may need a semicolon at the end to execute. Other environments are more flexible in terms of this being a "requirement." It is considered best practice to put a semicolon at the end of each statement, which also allows you to run multiple queries at once if your environment allows this.

**Best Practice**
```
SELECT account_id
FROM orders;
```
#### LIMIT clause
The `LIMIT` command is always the very last part of a query. An example of showing just the first 10 rows of the orders table with all of the columns might look like the following:
```
SELECT *
FROM orders
LIMIT 10;
```
#### ORDER BY clause
The `ORDER BY` statement allows us to sort our results using the data in any column. If you are familiar with Excel or Google Sheets, using ORDER BY is similar to sorting a sheet using a column.

The O`RDER BY` statement always comes in a query after the `SELECT` and `FROM` statements, but before the `LIMIT` statement. If you are using the `LIMIT` statement, it will always appear last.

Remember `DESC` can be added after the column in your ORDER BY statement to sort in descending order, as the default is to sort in ascending order.
```
SELECT id, account_id, total_amt_usd
FROM orders
ORDER BY total_amt_usd DESC
LIMIT 5;
```

- Compare the following:

Query 1:
  ```
  SELECT id, account_id, total_amt_usd
  FROM orders
  ORDER BY account_id, total_amt_usd DESC;
  ```
Query 2:
  ```
  SELECT id, account_id, total_amt_usd
  FROM orders
  ORDER BY total_amt_usd DESC, account_id;
  ```

In query #1, all of the orders for each account ID are grouped together, and then within each of those groupings, the orders appear from the greatest order amount to the least. In query #2, since you sorted by the total dollar amount first, the orders appear from greatest to least regardless of which account ID they were from. Then they are sorted by account ID next. (The secondary sorting by account ID is difficult to see here, since only if there were two orders with equal total dollar amounts would there need to be any sorting by account ID.)

#### WHERE clause
Using the WHERE statement, we can display subsets of tables based on conditions that must be met. You can also think of the WHERE command as filtering the data.

Common symbols used in WHERE statements include:
```
> (greater than)
< (less than)
>= (greater than or equal to)
<= (less than or equal to)
= (equal to)
!= (not equal to)
```
```
SELECT *
FROM orders
WHERE gloss_amt_usd >= 1000
LIMIT 5;
```

Using `WHERE` with non-numeric data:
```
SELECT name, website, primary_poc
FROM accounts
WHERE name = 'Exxon Mobil';
```

#### Derived Columns
Creating a new column that is a combination of existing columns is known as a derived column (or "calculated" or "computed" column). Usually you want to give a name, or "alias," to your new column using the `AS` keyword.

This derived column, and its alias, are generally only temporary, existing just for the duration of your query. The next time you run a query and access this table, the new column will not be there.

If you are deriving the new column from existing columns using a mathematical expression, then these familiar mathematical operators will be useful:
```
* (Multiplication)
+ (Addition)
- (Subtraction)
/ (Division)
```
Consider:
```
SELECT id, (standard_amt_usd/total_amt_usd)*100 AS std_percent, total_amt_usd
FROM orders
LIMIT 10;
```

#### Logical Operators
+ LIKE This allows you to perform operations similar to using WHERE and =, but for cases when you might not know exactly what you are looking for.

+ IN This allows you to perform operations similar to using WHERE and =, but for more than one condition.

+ NOT This is used with IN and LIKE to select all of the rows NOT LIKE or NOT IN a certain condition.

+ AND & BETWEEN These allow you to combine operations where all combined conditions must be true.

+ OR This allows you to combine operations where at least one of the combined conditions must be true.

#### LIKE operator
The LIKE operator is extremely useful for working with text. You will use LIKE within a WHERE clause. The LIKE operator is frequently used with %. The % tells us that we might want any number of characters leading up to a particular set of characters or following a certain set of characters.

```
SELECT name
FROM accounts
WHERE name LIKE 'C%';

or

SELECT name
FROM accounts
WHERE name LIKE '%one%';

or

SELECT name
FROM accounts
WHERE name LIKE '%s';
```

#### IN operator
The `IN` operator is useful for working with both numeric and text columns. This operator allows you to use an =, but for more than one item of that particular column. We can check one, two or many column values for which we want to pull data, but all within the same query. In the upcoming concepts, you will see the OR operator that would also allow us to perform these tasks, but the IN operator is a cleaner way to write these queries.

```
SELECT name, primary_poc, sales_rep_id
FROM accounts
WHERE name IN ('Walmart', 'Target', 'Nordstrom');
```

#### NOT operator
The `NOT` operator is an extremely useful operator for working with the previous two operators we introduced: IN and LIKE. By specifying NOT LIKE or NOT IN, we can grab all of the rows that do not meet a particular criteria.
```
SELECT name, primary_poc, sales_rep_id
FROM accounts
WHERE name NOT IN ('Walmart', 'Target', 'Nordstrom');

or

SELECT name
FROM accounts
WHERE name NOT LIKE 'C%';
```

#### AND and BETWEEN operators
The AND operator is used within a WHERE statement to consider more than one logical clause at a time. Each time you link a new statement with an AND, you will need to specify the column you are interested in looking at. You may link as many statements as you would like to consider at the same time. This operator works with all of the operations we have seen so far including arithmetic operators (+, *, -, /). LIKE, IN, and NOT logic can also be linked together using the AND operator.

Sometimes we can make a cleaner statement using BETWEEN than we can using AND. Particularly this is true when we are using the same column for different parts of our AND statement. In the previous video, we probably should have used BETWEEN.
```
SELECT *
FROM orders
WHERE standard_qty > 1000 AND poster_qty = 0 AND gloss_qty = 0;

or

SELECT name
FROM accounts
WHERE name NOT LIKE 'C%' AND name LIKE '%s';

or

SELECT occurred_at, gloss_qty
FROM orders
WHERE gloss_qty BETWEEN 24 AND 29;
(note: inclusive - both 24 and 29 are included in the query)

SELECT *
FROM web_events
WHERE channel IN ('organic', 'adwords') AND occurred_at BETWEEN '2016-01-01' AND '2017-01-01'
ORDER BY occurred_at DESC;
(note: it assumes the time is at 00:00:00 (i.e. midnight) for dates. This is the reason why we set the right-side endpoint of the period at '2017-01-01'.)
```

#### OR
Similar to the AND operator, the OR operator can combine multiple statements. Each time you link a new statement with an OR, you will need to specify the column you are interested in looking at. You may link as many statements as you would like to consider at the same time. This operator works with all of the operations we have seen so far including arithmetic operators (+, *, -, /), LIKE, IN, NOT, AND, and BETWEEN logic can all be linked together using the OR operator.

```
SELECT id
FROM orders
WHERE gloss_qty > 4000 OR poster_qty > 4000;

or

SELECT *
FROM orders
WHERE standard_qty = 0 AND (gloss_qty > 1000 OR poster_qty > 1000);

or

SELECT *
FROM accounts
WHERE (name LIKE 'C%' OR name LIKE 'W%')
           AND ((primary_poc LIKE '%ana%' OR primary_poc LIKE '%Ana%')
           AND primary_poc NOT LIKE '%eana%');

```

#### SQL query command order matters:
The order of the key words matter!
```
SELECT col1, col2
FROM table1
WHERE col3  > 5 AND col4 LIKE '%os%'
ORDER BY col5
LIMIT 10;
```

