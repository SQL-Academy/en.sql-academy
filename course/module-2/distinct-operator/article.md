---
meta:
    title: 'Duplicate elimination, operator DISTINCT'
    description: 'Examples of what the SQL DISTINCT operator is needed for, excluding repetitions in SQL for one or more columns.'
---

# Duplicate elimination, DISTINCT

In some situations, an SQL query for selecting data may return duplicate rows.

For example, let's retrieve the `class` field from the `Student_in_class` table in the database, where information about the school schedule is stored.

```sql
SELECT class FROM Student_in_class;
```

| class |
| ----- |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 9     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 8     |
| 6     |
| 6     |
| 6     |
| 6     |
| 6     |
| 6     |
| 6     |
| 6     |
| 6     |
| 6     |
| 5     |
| 5     |
| 5     |
| 5     |
| 5     |
| 5     |
| 5     |
| 5     |
| 4     |
| 4     |
| 4     |
| 4     |
| 4     |
| 4     |
| 4     |
| 4     |
| 4     |
| 3     |
| 3     |
| 3     |
| 3     |
| 3     |
| 3     |
| 3     |
| 3     |
| 2     |
| 2     |
| 2     |
| 2     |
| 2     |
| 2     |
| 2     |
| 1     |
| 1     |
| 1     |
| 1     |
| 1     |
| 1     |
| 1     |

Since there may be several students in one class, it is not surprising that when outputting data, we can observe identical values.
To avoid such duplication when selecting data, there is the `DISTINCT` operator.

## Operator syntax

```sql
SELECT [DISTINCT] table_fields FROM table_name;
```

So in our case, the query to retrieve unique classes that have at least one student would look like this:

```sql
SELECT DISTINCT class FROM Student_in_class;
```

| class |
| ----- |
| 9     |
| 8     |
| 7     |
| 6     |
| 5     |
| 4     |
| 3     |
| 2     |
| 1     |

## DISTINCT for multiple column

When using the `DISTINCT` operator for two or more columns, records that have identical values in all fields will be removed.

So for such a table:

| first_name | last_name |
| ---------- | --------- |
| John       | Scott     |
| William    | Dawson    |
| Raul       | Hartman   |
| William    | Hartman   |
| John       | Scott     |
| John       | Hartman   |

a query with the `DISTINCT` operator would return all combinations of first and last names except for the duplicate "John Scott".

```sql
SELECT DISTINCT first_name, last_name FROM User;
```

| first_name | last_name |
| ---------- | --------- |
| John       | Scott     |
| William    | Dawson    |
| Raul       | Hartman   |
| William    | Hartman   |
| John       | Hartman   |
