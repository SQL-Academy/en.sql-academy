---
meta:
    title: "Selection restriction, LIMIT operator"
    description: "Description and syntax of the SQL limit statement, usage examples, and tasks for self-checking."
---

# Selection restriction, LIMIT operator

The `LIMIT` operator allows you to extract a specific range of records from one or more tables.

## General query structure with the LIMIT operator

**MySQL**

**Comma syntax:**

```sql
SELECT table_fields
FROM table_name
LIMIT [skip_count,] row_count;
```

- `table_fields` — the columns to include in the result.
- `table_name` — the table to select data from.
- `skip_count` (optional) — how many rows to skip before the output starts. If omitted, the output starts from the
  first row.
- `row_count` — how many rows to return. If the result has fewer rows, all of them are returned.

**OFFSET syntax:**

```sql
SELECT table_fields
FROM table_name
LIMIT row_count [OFFSET skip_count];
```

- `table_fields` — the columns to include in the result.
- `table_name` — the table to select data from.
- `row_count` — how many rows to return. If the result has fewer rows, all of them are returned.
- `skip_count` (optional) — how many rows to skip before the output starts. If omitted, the output starts from the
  first row.

**PostgreSQL**

```sql
SELECT table_fields
FROM table_name
LIMIT row_count [OFFSET skip_count];
```

- `table_fields` — the columns to include in the result.
- `table_name` — the table to select data from.
- `row_count` — how many rows to return. If the result has fewer rows, all of them are returned.
- `skip_count` (optional) — how many rows to skip before the output starts. If omitted, the output starts from the
  first row.

## Examples

Let's take a `Company` table:

| id  | name       |
| --- | ---------- |
| 1   | Don_avia   |
| 2   | Aeroflot   |
| 3   | Dale_avia  |
| 4   | air_France |
| 5   | British_AW |

To output lines 3 to 5, you need to use this request:

**MySQL**

```sql
SELECT * FROM Company LIMIT 2, 3;
```

Or what's the same thing:

```sql
SELECT * FROM Company LIMIT 3 OFFSET 2;
```

**PostgreSQL**

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

Now try it yourself ⚡️

The interactive demonstration is available [in the SQL Academy lesson](https://sql-academy.org/en/guide/limit).
