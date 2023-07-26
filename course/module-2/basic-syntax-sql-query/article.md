# Basic SQL query syntax

One of the main functions of SQL is to get data samples from the DBMS.
To do this, SQL uses the `SELECT` operator. Let's look at a few simple queries with his participation.

## Output of arbitrary values

To begin with, it is important to understand that using the `SELECT` operator, you can output data not only from database tables,
but also arbitrary rows, numbers, dates, etc. For example, this way you can output an arbitrary string:

```sql
SELECT "Hello world"
```

## Output of all data from the table

The `*` symbol is used to output all fields from a specific table. Let's take a look at the database schema and
output the data from one of the tables.

```sql
SELECT * FROM FamilyMembers
```

## Output of data from certain columns of the table

If it is necessary to display information only on certain columns of the table, and not all at once, then
this can be done by listing the column names separated by commas:

```sql
SELECT member_id, member_name FROM FamilyMembers
```

## Aliases

In case we want to output some columns of the table, but so that they are named differently in the final selection,
we can use aliases.

Their syntax is quite simple, we have to use the `AS` operator. As in the example below:

```sql
SELECT member_id, member_name AS Name FROM FamilyMembers
```

| member_id | Name              |
| --------- | ----------------- |
| 1         | Headley Quincey   |
| 2         | Flavia Quincey    |
| 3         | Andie Quincey     |
| 4         | Lela Quincey      |
| 5         | Annie Quincey     |
| 6         | Ernest Forrest    |
| 7         | Constance Forrest |

Or you can do without it by simply writing the desired field name separated by a space.

```sql
SELECT member_id, member_name Name FROM FamilyMembers
```

> Aliases can contain up to 255 characters (including spaces, numbers, and special characters)

## Tasks for self-testing

This is our first lesson of the practical module. Before that, there were only theoretical ones aimed at filling potential gaps in the theory of relational databases.
After each practical lesson, we offer a group of tasks for self-testing work in order to immediately practise the information received.

If you missed the module "Introduction", namely the article <a href="https://sql-academy.org/guide/intro-structure-of-course" target="_blank"> "Course structure"</a>, which described the principle of operation and interface of the block
"Tasks for self-testing", then we recommend <a href="https://sql-academy.org/guide/intro-structure-of-course" target="_blank"> to return to it</a>.
