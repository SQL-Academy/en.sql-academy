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

- `database_name` — the name of the new database.

**MySQL**

Database names usually use letters, numbers, and the characters "\_" and "$". The maximum name length is 64 characters.

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

**PostgreSQL**

If a database name is written without double quotes, it must start with a letter or the "\_" character. After that, you can use letters, numbers, as well as the "\_" and "$" characters. The maximum name length is 63 characters.

You can control the creation of the database using an SQL query:

```sql
SELECT datname FROM pg_database WHERE datistemplate = false;
```

| datname         |
| --------------- |
| user_database_1 |
| user_database_2 |
| postgres        |

> Note that in addition to user databases, PostgreSQL also contains service databases: postgres, template0, template1.

## Deleting database

Deleting a database is done using the `DROP DATABASE` operator:

```sql
DROP DATABASE database_name;
```

- `database_name` — the database to delete together with all its tables.

**PostgreSQL**

> You cannot drop a database if the current session is connected to it. The command will also fail if other active sessions are connected to that database.

**MySQL**

## IF EXISTS and IF NOT EXISTS

When creating or deleting a database, an error may occur. For example, the database may already exist or, on the contrary, may not exist yet. In such cases, the `IF EXISTS` and `IF NOT EXISTS` constructions are used.

That is, if we want to create a database only on the condition that it does not exist yet, then we use following syntax:

```sql
CREATE DATABASE IF NOT EXISTS database_name;
```

- `IF NOT EXISTS` (keyword) — create the database only if there is no database with this name yet. Otherwise the
  query does nothing and doesn't fail.
- `database_name` — the name of the new database.

If we want to delete the database only if it exists, then we use following syntax:

```sql
DROP DATABASE IF EXISTS database_name;
```

- `IF EXISTS` (keyword) — delete the database only if it exists. Otherwise the query does nothing and doesn't
  fail.
- `database_name` — the database to delete.

**PostgreSQL**

## IF EXISTS

When deleting a database, an error may occur if such a database does not exist. In this case, you can use the `IF EXISTS` construction.

When deleting a database, you can use the `IF EXISTS` construction:

```sql
DROP DATABASE IF EXISTS database_name;
```

- `IF EXISTS` (keyword) — delete the database only if it exists. Otherwise the query does nothing and doesn't
  fail.
- `database_name` — the database to delete.

## Test yourself

The `shop` database has tables with data. What happens when you run this query?

```sql
DROP DATABASE shop;
```

1. The query fails: the tables have to be dropped first — There is no need to empty the database first: DROP DATABASE deletes it together with all its tables.

2. **Correct answer:** The database is deleted together with all its tables and data — DROP DATABASE deletes the whole database. After that, the tables and data can only be restored from a backup.

3. The tables are deleted, but the empty database remains — DROP DATABASE deletes not only the contents but the database itself: after the query it is no longer in the list of databases.

There is no `shop` database on the server. What happens when you run this query?

```sql
DROP DATABASE IF EXISTS shop;
```

1. The query fails because the database does not exist — This is how DROP DATABASE behaves without IF EXISTS. The IF EXISTS clause is there exactly to avoid this error.

2. **Correct answer:** The query does nothing and does not fail — IF EXISTS deletes the database only if it exists, otherwise it skips the deletion without an error.

3. An empty shop database is created — DROP DATABASE never creates databases, and IF EXISTS only checks whether the database exists before deleting it.
