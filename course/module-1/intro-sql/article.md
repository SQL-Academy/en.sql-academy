---
meta:
  title: "Introduction to SQL"
  description: "Defining the SQL language. Dialects (extensions) of SQL. Differences between T-SQL, PL/SQL, PL/pgSQL."
---

# Introduction to SQL

> SQL is a structured query language that is used as an efficient way to store data, search for its parts, update, extract from the database, and delete.

Communication with relational DBMS happens in SQL. It is used to perform all the main database operations:

**Data**

- `SELECT` — extract data from a database
- `INSERT` — insert records into a database
- `UPDATE` — update records in a database
- `DELETE` — delete records from a database

**Structure**

- `CREATE DATABASE` — create new databases
- `CREATE TABLE` — create new tables in a database

**Logic and access**

- `CREATE PROCEDURE` — create stored procedures
- `CREATE VIEW` — create views
- `GRANT` — set permissions for tables, procedures, and views

## SQL dialects (SQL extensions)

SQL is a universal language for all relational database management systems,
but many DBMS make changes to the language they use, thus deviating from the standard. Such languages are called dialects or extensions of the language.

Here are some of them:

- T-SQL - Microsoft SQL Server dialect
- PL/SQL - Oracle Database dialect
- PL/pgSQL - PostgreSQL dialect

### Which dialect to learn?

If you know that you need to learn SQL, you should learn standard SQL. However, if you already know which specific database you will be working with, it is probably best to learn its SQL dialect and just know that different databases may use slightly different syntax.

In our course, we offer you a choice between MySQL DBMS and PostgreSQL DBMS, as the 2 most popular solutions.
You can pick a DBMS and switch it at any time in one of the following ways:

- in the gear icon menu in the site header
- in the code editor interface

  ![SQL code editor interface](https://sql-academy.org/static/guidePage/intro-sql/en_changing_dbms.png "SQL code editor interface")
