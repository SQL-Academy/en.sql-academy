---
meta:
  title: "Introduction to SQL"
  description: "Defining the SQL language. Dialects (extensions) of SQL. Differences between T-SQL, PL/SQL, PL/pgSQL."
---

# Introduction to SQL

> SQL is a structured query language that is used as an efficient way to store data, search for its parts, update, extract from the database, and delete.

Access to relational DBMS is made possible thanks to SQL. All major manipulations with databases are performed with it, some of them:

- Extract data from a database
- Insert records into the database
- Update records in the database
- Delete records from the database
- Create new databases
- Create new tables in the database
- Create stored procedures in the database
- Create views in the database
- Set permissions for tables, procedures, and views

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
You can switch between them at any time using one of the following methods:

- select DBMS on the <a href="/settings/profile" target="_blank">profile settings</a> page
- select DBMS in the code editor interface

  ![SQL code editor interface](https://sql-academy.org/static/guidePage/intro-sql/en_changing_dbms.png "SQL code editor interface")
