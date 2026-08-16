---
meta:
    title: "Introduction to SQL"
    description: "Defining the SQL language. Dialects (extensions) of SQL. Differences between T-SQL, PL/SQL, PL/pgSQL."
---

# Introduction to SQL

> SQL is a structured query language that is used as an efficient way to store data, search for its parts, update, extract from the database, and delete.

Communication with relational DBMS happens in SQL. It is used to perform all the main database operations:

**Data**

- `SELECT` — Extract data from a database
- `INSERT` — Insert records into a database
- `UPDATE` — Update records in a database
- `DELETE` — Delete records from a database

**Structure**

- `CREATE DATABASE` — Create new databases
- `CREATE TABLE` — Create new tables in a database

**Logic and access**

- `CREATE PROCEDURE` — Create stored procedures
- `CREATE VIEW` — Create views
- `GRANT` — Set permissions for tables, procedures, and views

## SQL dialects (SQL extensions)

SQL is a universal language for all relational database management systems,
but many DBMS make changes to the language they use, thus deviating from the standard. Such languages are called dialects or extensions of the language.

Here are some of them. As an example, let's see how each of these DBMS selects the first 3 rows.

| Dialect  | DBMS                 | Example             |
| -------- | -------------------- | ------------------- |
| T-SQL    | Microsoft SQL Server | `SELECT TOP 3 name` |
| PL/SQL   | Oracle Database      | `WHERE ROWNUM <= 3` |
| MySQL    | MySQL                | `name LIMIT 3`      |
| PL/pgSQL | PostgreSQL           | `name LIMIT 3`      |

### Which dialect to learn?

If you know that you need to learn SQL, you should learn standard SQL. However, if you already know which specific database you will be working with, it is probably best to learn its SQL dialect and just know that different databases may use slightly different syntax.

In our course, we offer you a choice between MySQL DBMS and PostgreSQL DBMS, as the 2 most popular solutions.
Try picking one right now — every example and exercise in the course will adapt to your choice:

**MySQL**

- The web classic: easy to start, running everywhere
- The most widespread open-source DBMS in the world
- Powers WordPress, YouTube and Booking.com

**PostgreSQL**

- Power and precision: the startup favourite
- Rich syntax and strict adherence to the SQL standard
- Chosen by Instagram, Spotify and Reddit

You can switch the DBMS at any time — right here, in the gear icon menu in the site header or in the code editor.
