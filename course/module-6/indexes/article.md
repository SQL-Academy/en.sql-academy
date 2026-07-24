---
meta:
    title: "SQL Indexes: speeding up data search in MySQL and PostgreSQL"
    description: "A comprehensive guide to SQL indexes, explaining how they simplify and speed up data search in database tables in MySQL and PostgreSQL. Learn how to create, manage, and optimize indexes to improve the performance of your queries."
---

![Article banner](https://sql-academy.org/static/guidePage/indexes/banner.jpg)# SQL IndexesWhen you insert a row into a table, the database server does not attempt to place the data
in any specific, particular place in the table. For example, if
you add a row to the `Users` table, the server does not arrange the rows in
numerical order of the `id` column values or in alphabetical order of the `last_name` column values.
Instead, it just puts the data in the next available space in the file (the server maintains a list of free spaces for each table).This leads to the database server having to check every row in the table to find matches when executing a query like:`sql
SELECT email FROM Users WHERE email LIKE 'l%';
`This is suitable for small tables, but becomes excessively time-consuming as the data volume grows.For comparison, how a search request for `email` performs depending on the presence of an index on the field.![Comparison of search speed in a table with an index](https://sql-academy.org/static/guidePage/indexes/statistic_en.png "Comparison of search speed in a table with an index")Indexes function like subject indexes in a book 📖, allowing you to quickly find information
without reading the entire text. They are special
tables that, unlike regular data tables, are stored in a certain order.
But instead of containing all the data about a
record, an index contains only the column (or columns) used to
find rows in the data table, along with information describing where
the row is physically located. Thus, the role of indexes is
to facilitate the search for subsets of rows and columns of a table without
the need to scan every row in the table.## Creating an Index ✨Returning to the `Users` table, you can add
an index to the `email` column to speed up any queries that work
with the value of this column.**MySQL**Here's how you can add such an index to a MySQL database:`sql
CREATE INDEX idx_email
    ON Users (email);
`This statement creates an index for the `Users.email` column. This index is named `idx_email`.
With an index present, the query optimizer can choose to use the index if
it considers it useful. If there are more than one index in the table, the optimizer must decide which index is most beneficial
for a specific SQL statement.All database management systems provide the ability to view existing indexes.
For MySQL users, there is a `SHOW` command that allows you to display all indexes
for a specific table, as shown in the example below:```sql
SHOW INDEX FROM Users;

````| Table | Non_unique | Key_name  | Seq_in_index | Column_name |
| ----- | ----------- | ---------- | -------------- | ------------ |
| users | 0           | PRIMARY    | 1              | id           |
| users | 1           | idx\_email | 1              | email        |The output shows that there are 2 indexes in the `Users` table: one for the `id` column named `PRIMARY`
and another for the `email` column that we just defined.When the table was created, the MySQL server automatically generated
an index for the primary key column, which in this case is
`id`, and named the index `PRIMARY`.
This is a special type of index used with the primary key constraint, which ensures
that each value in the column or group of columns designated as the table's primary key
is unique and cannot be `NULL`.**PostgreSQL**Here's how you can add such an index to a PostgreSQL database:```sql
CREATE INDEX idx_email
    ON Users (email);
```This statement creates an index for the `Users.email` column. This index is named `idx_email`.
With an index present, the query optimizer can choose to use the index if
it considers it useful. If there are more than one index in the table, the optimizer must decide which index is most beneficial
for a specific SQL statement.All database management systems provide the ability to view existing indexes.
For PostgreSQL users, you can use a query to system tables to display all indexes
for a specific table:```sql
SELECT indexname, indexdef
FROM pg_indexes
WHERE tablename = 'users';
```| indexname   | indexdef                                                         |
| ----------- | ---------------------------------------------------------------- |
| users\_pkey | CREATE UNIQUE INDEX users\_pkey ON public.users USING btree (id) |
| idx\_email  | CREATE INDEX idx\_email ON public.users USING btree (email)      |PostgreSQL automatically creates an index for the primary key column, which in this case is
`id`. This is a special type of index used with the primary key constraint, which ensures
that each value in the column or group of columns designated as the table's primary key
is unique and cannot be `NULL`.PostgreSQL supports various index types:* **B-tree** (default) — for comparison and sorting operations
* **Hash** — for equality operations
* **GIN** — for composite values (arrays, JSON)
* **GiST** — for geometric data and full-text search
* **BRIN** — for very large tables with natural sorting## Dropping an Index**MySQL**If after creating an index you decide that the index is no longer needed, you can
remove it as follows:```sql
DROP INDEX idx_email ON Users;
```**PostgreSQL**If after creating an index you decide that the index is no longer needed, you can
remove it as follows:```sql
DROP INDEX idx_email;
```## Unique IndexesWhen designing databases, it's important to determine which columns can have duplicate values
and which cannot.For example, in the `Users` table, there may be several users with the same names,
but identifiers and email addresses must be different to
distinguish between them.Guaranteed uniqueness of values can be achieved by creating a unique index on the `Users.email` column.
A unique index performs two functions:* it provides all the benefits of a standard index
* it prevents duplication of values in the indexed columnThe database management system will check the unique index when attempting to add or
modify data in the indexed column to ensure that the entered value does not duplicate
an existing one in the table.Creating a unique index for the `Users.email` column is done as follows:```sql
CREATE UNIQUE INDEX idx_email
    ON Users (email);
```**MySQL**With the index present, you will receive an error message if you attempt to add a new customer with an already existing email address:```sql
Error(1062) 23000: "Duplicate entry 'duplicate@gmail.com' for key 'users.idx_email'"
```**PostgreSQL**With the index present, you will receive an error message if you attempt to add a new customer with an already existing email address:```sql
ERROR: duplicate key value violates unique constraint "idx_email"
DETAIL: Key (email)=(duplicate@gmail.com) already exists.
```Creating unique indexes for the column or columns defined as the primary key is redundant,
as the database management system automatically ensures the uniqueness of the primary key values.
However, placing several unique indexes in one table is permissible and may be sensible
if you see the need for it.## Multi-Column IndexesIn addition to single-column indexes, it is possible to create indexes
that include several columns.
For example, to search for students by `first_name` and `last_name`, you can create a composite index
for these two fields:```sql
CREATE INDEX idx_full_name
    ON Student (last_name, first_name);
```Such an index will be useful for queries that require both the last name and the first name, or just the last name.
However, it will not be beneficial for queries that specify only the first name.
This is similar to searching for a phone number in a telephone directory: if both the first name and last name are known,
the search is simplified thanks to the directory being organized by last name and then by first name.
If only the first name is known, you have to go through all the entries to find the right person.When creating indexes that include several columns, it's important to think about the order of the columns in the index
to make it as effective as possible.
Nonetheless, if necessary for achieving the required query performance, it's possible to create multiple indexes with the same columns but in a different order.## How indexes are usedIndexes are often used by the database server for efficiently finding the necessary rows in a table,
and then for retrieving additional data from related tables upon user request.
Take, for example, the query:```sql
SELECT id, first_name, last_name
  FROM Student
  WHERE first_name LIKE 'A%' AND last_name LIKE 'L%'
```In response to such a query, the server can choose one of several approaches:* Perform a full scan of all rows in the table.
* Use the index on the `last_name` column to find students with a last name starting with 'L', and then check each of these rows for a first name starting with 'A'.
* Use a composite index on `last_name` and `first_name` to directly find students meeting both criteria.The last method appears to be the most efficient, as it allows all necessary rows to be found
in one pass, avoiding revisiting the table.**MySQL**But how to determine which method the MySQL query optimizer will choose?
This can be done using the `EXPLAIN` command, which shows how the server plans
to execute the query without actually running it:```sql
EXPLAIN
  SELECT id, first_name, last_name
  FROM Student
  WHERE first_name LIKE 'A%'
  AND last_name LIKE 'L%';
```| id | select\_type | table   | partitions | possible\_keys                  | key             |
| -- | ------------ | ------- | ---------- | ------------------------------- | --------------- |
| 1  | SIMPLE       | Student |            | idx\_full\_name,idx\_last\_name | idx\_full\_name |Analyzing the results, it can be seen that the `possible_keys` column lists potentially
applicable indexes `idx_last_name` or `idx_full_name`, and the `key` column indicates
that the `idx_full_name` index was chosen.**PostgreSQL**But how to determine which method the PostgreSQL query optimizer will choose?
This can be done using the `EXPLAIN` command, which shows how the database plans
to execute the query:```sql
EXPLAIN
  SELECT id, first_name, last_name
  FROM Student
  WHERE first_name LIKE 'A%'
  AND last_name LIKE 'L%';
```| QUERY PLAN                                                                     |
| ------------------------------------------------------------------------------ |
| Index Scan using idx\_full\_name on student  (cost=0.42..8.44 rows=1 width=68) |
|   Index Cond: ((last\_name >= 'L'::text) AND (last\_name \< 'M'::text))        |
|   Filter: ((first\_name \~\~ 'A%'::text) AND (last\_name \~\~ 'L%'::text))     |You can also use `EXPLAIN ANALYZE` to get real execution statistics:```sql
EXPLAIN ANALYZE
  SELECT id, first_name, last_name
  FROM Student
  WHERE first_name LIKE 'A%'
  AND last_name LIKE 'L%';
```| QUERY PLAN                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------ |
| Index Scan using idx\_full\_name on student  (cost=0.42..8.44 rows=1 width=68) (actual time=0.025..0.027 rows=1 loops=1) |
|   Index Cond: ((last\_name >= 'L'::text) AND (last\_name \< 'M'::text))                                                  |
|   Filter: ((first\_name \~\~ 'A%'::text) AND (last\_name \~\~ 'L%'::text))                                               |
| Planning Time: 0.156 ms                                                                                                  |
| Execution Time: 0.048 ms                                                                                                 |Analyzing the `EXPLAIN` results, you can see which access method the optimizer chose —
full table scan (Seq Scan) or index scan (Index Scan).## The Flip Side of IndexesIf indexes are so effective, one might ask: why not just index everything? 🧐The answer lies in the fact that each index represents a table (although a special type
of table, but still a table). Consequently, every time a row
is added to a table or removed from it, all indexes in that table must be updated. When updating a row, any indexes for the column
(or columns) that were affected also need to be changed.
Therefore, the more indexes you have, the more the server has to work to keep all schema objects up to date — which
leads to slower performance.Moreover, indexes occupy additional disk space and require careful management
by database administrators. Therefore, the optimal solution is to create indexes
only when truly necessary. If an index is needed temporarily, for example, for
running a monthly report, it can be added before the procedure starts and removed after its
completion. In situations where nightly data loads interfere with index operations, they are
temporarily removed before loading and restored afterward to avoid disrupting daytime operations.In the end, the ideal approach is to find a balance: it's necessary to have enough indexes
for efficient operation, but not so many that it impacts performance.
If you're unsure about the right number of indexes, start with the minimum and add
as necessary.**Let's check how well we've understood the topic:**Which statement best explains why you should not index every column in a database table?1) Indexes increase the speed of all operations in the database, including inserting, updating, and deleting data. — While indexes can indeed speed up read query executions, they can slow down insert, update, and delete operations, as they require additional updates to the index structures.

2) Indexes do not require additional disk space and therefore can be created without any drawbacks. — Indexes take up additional disk space, which is one of the reasons why not every column should be indiscriminately indexed.

3) Indexes reduce the need for query optimization, as they automatically optimize all queries. — Although indexes can significantly improve the performance of some queries, they do not replace the need for careful query optimization and database schema design.

4) **Correct answer:&#x20;**&#x49;ndexes speed up read operations but can slow down write operations, as each index needs to be updated when data changes. — This statement accurately reflects the trade-off associated with using indexes. They indeed speed up reading at the expense of potentially slowing down write operations, making it important to balance the number of indexes and their impact on the overall system performance.
````
