---
meta:
    title: "VIEW in SQL: MySQL and PostgreSQL"
    description: "Syntax for creating views in MySQL and PostgreSQL, description of how views work and why they are needed."
---

# Views

Well-designed applications usually provide an open interface, hiding implementation details,
which allows for design changes without affecting end users.

When developing your database, you can achieve a similar result by encapsulating tables and providing
access to data only through a set of views.

In this article, we will discuss what views are, how they are created, and what they can be useful for.

## What is a view

> A view is a database object that is the result of executing a query on the database,
> defined using the `SELECT` statement, at the time of accessing the view.

Views are sometimes called "virtual tables."
This is because a view appears to the user as a table, but actually does not store data,
but retrieves it from other tables at the time of access.

If the data in the underlying table changes, the user receives up-to-date data when accessing the view
that uses this table. Views do not cache query results during operation.

## Example of creating a view

As a simple example, suppose you want to partially hide email addresses in the user table (`Users`).

Airbnb database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/view).

This can be useful, for example, if your company's policy does not allow everyone to use
confidential user information.
Therefore, instead of allowing direct access to the user table (`Users`), you define
a view named `ViewUsers` and require everyone
to use it to access user data.

**MySQL**

Here is an example of defining this view:

```sql
CREATE VIEW ViewUsers AS
    SELECT id,
           name,
           CONCAT(SUBSTR(email, 1, 2), '****', SUBSTR(email, -4)) AS email
FROM Users;
```

**PostgreSQL**

Here is an example of defining this view:

```sql
CREATE VIEW ViewUsers AS
    SELECT id,
           name,
           CONCAT(SUBSTR(email, 1, 2), '****', RIGHT(email, 4)) AS email
FROM Users;
```

The view in an SQL query appears and is used like a regular table.

```sql
SELECT * FROM ViewUsers;
```

| id  | name                 | email          |
| --- | -------------------- | -------------- |
| 1   | Bruce Willis         | ba\*\*\*\*.com |
| 2   | George Clooney       | te\*\*\*\*.com |
| 3   | Kevin Costner        | me\*\*\*\*.com |
| 4   | Donald Sutherland    | ra\*\*\*\*.net |
| 5   | Jennifer Lopez       | ba\*\*\*\*.com |
| 6   | Ray Liotta           | jd\*\*\*\*.net |
| 7   | Samuel L. Jackson    | mo\*\*\*\*.com |
| 8   | Nikole Kidman        | ok\*\*\*\*.com |
| 9   | Alan Rickman         | ka\*\*\*\*.net |
| 10  | Kurt Russell         | ga\*\*\*\*.com |
| 11  | Harrison Ford        | ko\*\*\*\*.com |
| 12  | Russell Crowe        | gl\*\*\*\*.net |
| 13  | Steve Martin         | ne\*\*\*\*.com |
| 14  | Michael Caine        | dm\*\*\*\*.com |
| 15  | Angelina Jolie       | ch\*\*\*\*.net |
| 16  | Mel Gibson           | ro\*\*\*\*o.ca |
| 17  | Michael Douglas      | ti\*\*\*\*.com |
| 18  | John Travolta        | wa\*\*\*\*.com |
| 19  | Sylvester Stallone   | ko\*\*\*\*.com |
| 20  | Tommy Lee Jones      | sz\*\*\*\*.com |
| 21  | Catherine Zeta-Jones | fl\*\*\*\*.com |
| 22  | Antonio Banderas     | sa\*\*\*\*.net |
| 23  | Kim Basinger         | ja\*\*\*\*.net |
| 24  | Sam Neill            | cl\*\*\*\*.com |
| 25  | Hideo Kojima         | ad\*\*\*\*.org |
| 26  | ClINT Eastwood       | fr\*\*\*\*.net |
| 27  | Brad Pitt            | ke\*\*\*\*.net |
| 28  | Johnny Depp          | cg\*\*\*\*o.ca |
| 29  | Pierce Brosnan       | tr\*\*\*\*.com |
| 30  | Sean Connery         | js\*\*\*\*.com |
| 31  | Bruce Willis         | ke\*\*\*\*.com |
| 32  | Mullah Omar          | jg\*\*\*\*.com |
| 33  | Vasanta Roberta      | ro\*\*\*\*.com |

**MySQL**

If you want to find out which columns are available in the view, you can use the `DESCRIBE` statement.

```sql
DESCRIBE ViewUsers;
```

| Field | Type        | Null | Key | Default | Extra |
| ----- | ----------- | ---- | --- | ------- | ----- |
| id    | int         | NO   |     | \<NULL> |       |
| name  | varchar(32) | NO   |     | \<NULL> |       |
| email | varchar(38) | YES  |     | \<NULL> |       |

**PostgreSQL**

If you want to find out which columns are available in the view, you can use a query to `information_schema`:

```sql
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'viewusers';
```

## General syntax of a view

**MySQL**

```sql
CREATE [OR REPLACE]
VIEW view_name [(view_column_names)]
AS select_expression
```

`OR REPLACE` — When using this optional parameter, if a view with the same
name already exists, the old view will be deleted and a new one will be created. Otherwise, if you try to create
a view with an existing name, an error will occur.

**PostgreSQL**

```sql
CREATE [OR REPLACE] VIEW view_name [(view_column_names)]
AS select_expression
```

`OR REPLACE` — When using this optional parameter, if a view with the same
name already exists, the old view will be deleted and a new one will be created. Otherwise, if you try to create
a view with an existing name, an error will occur.

## Why are views needed

### Simplifying complex queries

Views are used to simplify complex queries and create an abstraction between the user and the database.
They can hide the complexity of data structures and provide a simplified interface for accessing data.

### Improving performance

**MySQL**

Creating views that encapsulate complex queries can help optimize the execution of these queries.
This can lead to faster query execution and overall improvement in database performance.

**PostgreSQL**

Creating views that encapsulate complex queries can help optimize the execution of these queries.
PostgreSQL supports materialized views (`MATERIALIZED VIEW`) that physically store query results
and are periodically updated, which can significantly improve performance for complex queries.

### Ensuring security

Views can be used to ensure the security of confidential data.
Creating views that restrict access to specific columns or rows of data
allows administrators to limit access to sensitive information.
This helps ensure that only authorized users have access to confidential data.

## Conclusion

Views are an important tool in SQL that allows for simplifying complex queries, standardizing data access, improving performance, and ensuring data security

Let's check how well you understood the topic: choose the correct statement for the question "What is a view in a database?"

1. **Correct answer: **A virtual table that does not store data but retrieves it from other tables when accessed. — A view in a database is indeed a virtual table that does not store data itself but retrieves it from other tables when accessed.

2. A physical table that permanently stores data. — No, a view is not a physical table that permanently stores data. It is a virtual table that retrieves data from other tables when accessed. Views do not store data on their own but provide access to data already stored in other tables.

3. An interface for direct user access to the database. — No, a view is not an interface for direct user access to the database. It hides implementation details and provides a simplified interface for data access. Users interact with the view, not the database directly.

4. A temporary table used for data manipulation. — No, a view is not a temporary table used for data manipulation. It is a virtual table that retrieves data from other tables when accessed. Views are not used for modifying data but for providing convenient access to it.
