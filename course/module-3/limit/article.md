---
meta:
    title: 'Selection restriction, LIMIT operator'
    description: 'Description and syntax of the SQL limit statement, usage examples, and tasks for self-checking.'
---

# Selection restriction, LIMIT operator

The `LIMIT` operator allows you to extract a specific a range of records from one or more tables.

## General query structure with the LIMIT operator

```sql
SELECT table_fields
FROM table_name
LIMIT [number_of_skipped_records,] number_of_records_to_output;
```

If you do not specify the number of skipped records, they will be counted from the beginning of the table.

> The `LIMIT` operator is not implemented in all DBMS, e.g. MSSQL to output entries from the beginning of the table is used the `TOP` operator,
> but for those occasions when you need to make the offset from beginning of table, designed design `OFFSET FETCH`.

## Examples

Let's take a Company table:

| id  | name       |
| --- | ---------- |
| 1   | Don_avia   |
| 2   | Aeroflot   |
| 3   | Dale_avia  |
| 4   | air_France |
| 5   | British_AW |

To output lines 3 to 5, you need to use this request:

```sql
SELECT * FROM Company LIMIT 2, 3;
```

Or what's the same thing:

```sql
SELECT * FROM Company LIMIT 3 OFFSET 2;
```

The query returns the following selection:

| id  | name       |
| --- | ---------- |
| 3   | Dale_avia  |
| 4   | air_France |
| 5   | British_AW |

This query skips the first two rows of the table (1, 2), and then outputs the next three entries (3, 4, 5).
