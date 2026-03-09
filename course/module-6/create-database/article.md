---
meta:
  title: "Creating and deleting databases: MySQL and PostgreSQL"
  description: "SQL syntax for creating and dropping databases in MySQL and PostgreSQL: CREATE DATABASE, DROP DATABASE, IF EXISTS, and IF NOT EXISTS."
---

# Creating and deleting databases

When writing SQL queries, we actively use tables. The tables themselves are stored within specific databases, which will be discussed in this article.

## Database creation

Database creation has the following syntax:

```sql
CREATE DATABASE database_name;
```

### MySQL

In MySQL, database names usually use letters, numbers, and the characters "\_" and "$". The maximum name length is 64 characters.

You can control the creation of the database using the `SHOW DATABASES` operator.

```sql
SHOW DATABASES;
```

> Note that the `SHOW DATABASES` operator, in addition to user databases, also displays service databases: information_schema, mysql, performance_schema, sys.

### PostgreSQL

In PostgreSQL, if a database name is written without double quotes, it must start with a letter or the "\_" character. After that, you can use letters, numbers, as well as the "\_" and "$" characters. The maximum name length is 63 characters.

You can control the creation of the database using an SQL query:

```sql
SELECT datname FROM pg_database WHERE datistemplate = false;
```

> Note that in addition to user databases, PostgreSQL also contains service databases: postgres, template0, template1.

## Deleting database

Deleting a database is done using the `DROP DATABASE` operator:

```sql
DROP DATABASE database_name;
```

> PostgreSQL: you cannot drop a database if the current session is connected to it. The command will also fail if other active sessions are connected to that database.

## IF EXISTS and IF NOT EXISTS

### MySQL

When creating or deleting a database, an error may occur. For example, the database may already exist or, on the contrary, may not exist yet. In such cases, the `IF EXISTS` and `IF NOT EXISTS` constructions are used.

If we want to create a database only on the condition that it does not exist yet, then we use the following syntax:

```sql
CREATE DATABASE IF NOT EXISTS database_name;
```

If we want to delete the database only if it exists, then we use the following syntax:

```sql
DROP DATABASE IF EXISTS database_name;
```

### PostgreSQL

When deleting a database, an error may occur if such a database does not exist. In this case, you can use the `IF EXISTS` construction.

In PostgreSQL, you can use the `IF EXISTS` construction when deleting a database:

```sql
DROP DATABASE IF EXISTS database_name;
```
