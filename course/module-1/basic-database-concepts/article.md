---
meta:
    title: "Databases and DBMS"
    description: "Definition of database and database management system. How databases work. Rating and popularity of relational databases. Features of relational databases. SQL query language."
---

# Databases and DBMS

Before we start learning SQL, let's get acquainted with the basic concepts
of databases. This will help us understand the scope of SQL and the environment it operates in.

> A database is a set of data stored in a structured way

In fact, this is just a repository of some information, nothing more. Databases themselves would be of no interest if there were no database management systems.

> A database management system is a set of software tools that provides
> access to data, allows it to be created, modified and deleted, ensures
> data security, and so on.

In simpler terms, a DBMS is a system that lets you
create databases and manipulate the information in them.

The simplest scheme for working with a database:

![Database operation scheme](https://sql-academy.org/static/guidePage/basic-database-concepts/en_schema_of_db_work.png "title")

## DBMS rating

At the moment, the rating of database management systems is as follows:

- `Oracle` - relational DBMS
- `MySQL` - relational DBMS
- `Microsoft SQL Server` - relational DBMS
- `PostgreSQL` - relational DBMS
- `MongoDB` - document-oriented databases
- `Redis` - key-value databases
- `Snowflake` - cloud relational DBMS
- `Elasticsearch` - search engine
- `IBM Db2` - relational DBMS
- `SQLite` - relational DBMS

It can be noted that 7 of the 10 most popular DBMS are relational. You made the right choice to study them 😉.

Let's check how we learned the topic, choose the correct statement:

1. The DBMS does not interact with the database in any way. — Database management systems (DBMS) allow you to manage, modify and delete databases. So the DBMS interacts with the database, the diagram above shows the scheme of their work.

2. **Correct answer:** The DBMS manages the data stored in the database — A database is just a collection of data. You really need a database management system to manage them.

3. To interact with all DBMS, you can use the SQL language — DBMS are different and not all of them use the SQL query language. SQL is the standard for relational DBMS.
