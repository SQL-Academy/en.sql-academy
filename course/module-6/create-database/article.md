---
meta:
  title: "Creating and deleting databases: MySQL and PostgreSQL"
  description: "SQL syntax for creating and dropping databases in MySQL and PostgreSQL, CREATE DATABASE, DROP DATABASE. IF NOT EXISTS construction."
---

# Creating and deleting databases

When writing SQL queries, we actively use tables. The tables themselves are stored within specific databases, which will be discussed in this article.

## Database creation

Database creation has the following syntax:

```sql
CREATE DATABASE database_name;
```

You can use combinations of letters, numbers, and the characters "\_" and "$" as the name for the database.

<MySQLOnly>

The name can start with numbers, but it cannot consist only of them. The maximum name length is 64 characters.

You can control the creation of the database using the `SHOW DATABASES` operator.

```sql
SHOW DATABASES;
```

> Note that the `SHOW DATABASES` operator, in addition to user databases, also displays service databases: information_schema, mysql, performance_schema, sys.

</MySQLOnly>

<PostgreSQLOnly>

The name cannot start with a digit and cannot contain special characters (except underscore). The maximum name length is 63 characters.

You can control the creation of the database using an SQL query:

```sql
SELECT datname FROM pg_database WHERE datistemplate = false;
```

> Note that in addition to user databases, PostgreSQL also contains service databases: postgres, template0, template1.

</PostgreSQLOnly>

## Deleting database

Deleting a database is done using the `DROP DATABASE` operator:

```sql
DROP DATABASE database_name;
```

## IF [NOT] EXISTS construction

When creating a database or deleting it, an error may occur that a database with such name already exists (on creation) or, conversely,
this database does not exist (on deletion). For such cases, there is a construction `IF [NOT] EXISTS`.

That is, if we want to create a database only on the condition that it does not exist yet, then we use following syntax:

```sql
CREATE DATABASE IF NOT EXISTS database_name;
```

If we want to delete the database only if it exists, then we use following syntax:

```sql
DROP DATABASE IF EXISTS database_name;
```
