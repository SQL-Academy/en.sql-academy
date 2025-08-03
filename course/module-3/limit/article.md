---
meta:
  title: "Selection restriction, LIMIT operator"
  description: "Description and syntax of the SQL limit statement, usage examples, and tasks for self-checking."
---

# Selection restriction, LIMIT operator

The `LIMIT` operator allows you to extract a specific a range of records from one or more tables.

## General query structure with the LIMIT operator

<MySQLOnly>

**Comma syntax:**

```sql
SELECT table_fields
FROM table_name
LIMIT [number_of_skipped_records,] number_of_records_to_output;
```

**OFFSET syntax:**

```sql
SELECT table_fields
FROM table_name
LIMIT number_of_records_to_output [OFFSET number_of_skipped_records];
```

If you do not specify the number of skipped records, they will be counted from the beginning of the table.

</MySQLOnly>

<PostgreSQLOnly>

```sql
SELECT table_fields
FROM table_name
LIMIT number_of_records_to_output [OFFSET number_of_skipped_records];
```

If you do not specify `OFFSET`, they will be counted from the beginning of the table.

</PostgreSQLOnly>

## Examples

Let's take a `Company` table:

To output lines 3 to 5, you need to use this request:

<MySQLOnly>

```sql-executable-Airo
SELECT * FROM Company LIMIT 2, 3;
```

Or what's the same thing:

```sql-executable-Airo
SELECT * FROM Company LIMIT 3 OFFSET 2;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Airo
SELECT * FROM Company LIMIT 3 OFFSET 2;
```

</PostgreSQLOnly>

The query returns the following selection:

This query skips the first two rows of the table (1, 2), and then outputs the next three entries (3, 4, 5).

Now try it yourself ⚡️
