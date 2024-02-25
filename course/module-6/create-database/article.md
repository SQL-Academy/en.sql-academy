---
meta:
    title: 'Creating and deleting databases'
    description: 'SQL syntax for creating and dropping databases, CREATE DATABASE, DROP DATABASE. IF NOT EXIST.'
---

# Creating and deleting databases

When writing SQL queries, we actively use tables. The tables themselves are stored within specific databases, which will be discussed in this article.

## Database creation

Database creation has the following syntax:

```sql
CREATE DATABASE database_name;
```

You can use combinations of letters, numbers, and the characters "\_" and "$" as the name for the database. The name can start with numbers, but it cannot consist only of them. The maximum name length is 64 characters.

You can control the creation of the database using the `SHOW DATABASES` operator.

```sql
SHOW DATABASES;
```

| Database           |
| ------------------ |
| user_table_1       |
| user_table_2       |
| information_schema |
| mysql              |
| performance_schema |
| sys                |

> Note that the `SHOW DATABASES` operator, in addition to user databases, also displays service databases: information_schema, mysql, performance_schema, sys.

## Deleting database

Deleting a database is done using the `DROP DATABASE` operator:

```sql
DROP DATABASE database_name;
```

## IF [NOT] EXIST construction

When creating a database or deleting it, an error may occur that a database with such name already exists (on creation) or, conversely,
this database does not exist (on deletion). For such cases, there is a construction `IF [NOT] EXIST`.

That is, if we want to create a database only on the condition that it does not exist yet, then we use following syntax:

```sql
CREATE DATABASE IF NOT EXIST database_name;
```

If we want to delete the database only if it exists, then we use following syntax:

```sql
DROP DATABASE IF EXIST database_name;
```
