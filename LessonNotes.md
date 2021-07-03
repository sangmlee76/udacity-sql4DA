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
