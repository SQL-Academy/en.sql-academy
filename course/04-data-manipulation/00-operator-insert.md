---
meta:
    title: "Adding data, INSERT operator"
    description: "How to add records to an SQL table with INSERT INTO: syntax, inserting several rows at once, auto-increment of the primary key. Examples for MySQL and PostgreSQL."
---

# Adding data, INSERT operator

So far we have only read data. But data in tables does not stand still: new products, customers and orders appear, and each of them needs a new row in a table. That is what the `INSERT` operator is for.

## General query structure with the INSERT operator

```sql
INSERT INTO table_name [(table_field [, ...])]
{ VALUES (value [, ...]) [, ...] | select_expression };
```

- `table_name` — the table that new records are added to.
- `table_field` (optional) — the columns that receive values. Without the list, you pass values for every column of
  the table.
- `value` — a value for a column: a number, a string, an expression, or `NULL`. Values go in the
  same order as the columns. Each pair of parentheses is one row; separate several rows
  with commas.
- `select_expression` (subquery) — a `SELECT` query whose result rows are added to the table. It must return as many
  columns as you listed.

Values are either listed after `VALUES` or returned by a `SELECT` query.

## Field enumeration and value correspondence

You can explicitly specify which fields of the table receive the data. To do this, list the field names in parentheses after the table name:

```sql
INSERT INTO table_name (table_field [, ...])
VALUES (value [, ...]);
```

- `table_name` — the table that new records are added to.
- `table_field` — a column that the value is written to. List columns separated by commas, in any order.
- `value` — a value for the column at the same position in the field list.

**Important rules:**

- The order of values in `VALUES` must strictly correspond to the order of fields in the enumeration
- The number of values must match the number of specified fields
- If a field is not specified in the enumeration, it will receive the default value (if one is set) or `NULL` (if the field allows `NULL`)

For example, if the `Goods` table has fields `good_id`, `good_name`, and `type`, then the following queries are equivalent:

```sql
-- Explicit field specification
INSERT INTO Goods (good_id, good_name, type)
VALUES (20, 'Table', 8);

-- Changed field order - values change accordingly
INSERT INTO Goods (good_name, type, good_id)
VALUES ('Table', 8, 20);
```

If you don't specify the list of fields, then values must be listed for **all** table fields in the order they were defined when creating the table:

```sql
INSERT INTO Goods
VALUES (20, 'Table', 8);
```

> It is recommended to always explicitly specify the list of fields.
> This makes the code more readable, protects against errors when changing the table structure, and allows inserting values only into the required fields.

## Differences between INSERT syntaxes

Let's add a dining table to the goods list. There are two ways to do it.

Family database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/operator-insert).

### INSERT INTO ... VALUES

The values are listed in parentheses after `VALUES`. This way suits data that is **known in advance**:

```sql
INSERT INTO Goods (good_id, good_name, type)
VALUES (20, 'Table', 8);
```

You can also add several rows at once, each in its own parentheses:

```sql
INSERT INTO Goods (good_id, good_name, type)
VALUES
    (20, 'Table', 8),
    (21, 'Chair', 8),
    (22, 'Lamp', 8);
```

### INSERT INTO ... SELECT

The values are returned by a `SELECT` query. It can even be written without a table, in which case it simply returns the listed values:

```sql
INSERT INTO Goods (good_id, good_name, type)
SELECT 20, 'Table', 8;
```

Both ways add the same row:

| good_id | good_name            | type  |
| ------- | -------------------- | ----- |
| 1       | apartment fee        | 1     |
| 2       | phone fee            | 1     |
| 3       | bread                | 2     |
| 4       | milk                 | 2     |
| 5       | red caviar           | 3     |
| 6       | cinema               | 4     |
| 7       | black caviar         | 3     |
| 8       | cough tablets        | 5     |
| 9       | potato               | 2     |
| 10      | pineapples           | 3     |
| 11      | television           | 8     |
| 12      | vacuum cleaner       | 8     |
| 13      | jacket               | 7     |
| 14      | fur coat             | 7     |
| 15      | music school fee     | 6     |
| 16      | english school fee   | 6     |
| 17      | airsoft game session | 4     |
| 18      | netflix subscription | 4     |
| **20**  | **Table**            | **8** |

More often, though, `INSERT ... SELECT` is used when the data **comes from the database**: to copy existing rows or insert calculation results. For example, let's duplicate all food products under new numbers:

```sql
INSERT INTO Goods (good_id, good_name, type)
SELECT good_id + 100, good_name, type
FROM Goods
WHERE type = 2;
```

## Primary key when adding a new record

A primary key value cannot repeat within a table: trying to insert an existing one ends with an error.

So which number is free? The first idea that comes to mind is to take the largest one and add one:

```sql
INSERT INTO Goods SELECT MAX(good_id) + 1, 'Table', 8 FROM Goods;
```

This works, but it is unreliable. If two such queries run at the same time, they can get the same number, and one of them fails with an error. Besides, the approach only suits numeric keys and can hand out a number that used to belong to a deleted record. If old data still refers to that number somewhere, it ends up linked to the new row 💥. That is why in real projects the database itself assigns the number.

## Automatic primary key generation

**MySQL**

The primary key can be generated automatically: give the `good_id` column the `AUTO_INCREMENT` attribute.
Then, when adding a record, pass `NULL` or `0` as `good_id`, and the field gets the next counter value: one greater than the largest value used so far.

```sql
CREATE TABLE Goods (
	good_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
	good_name VARCHAR(255),
	type INT
);
```

```sql
INSERT INTO Goods VALUES (NULL, 'Table', 8);
```

**PostgreSQL**

A unique identifier can be generated automatically with the `SMALLSERIAL`, `SERIAL`, and `BIGSERIAL` types.
They are not real types but a shorthand: a column with one of them is an integer that grows automatically when a new record is added.

```sql
CREATE TABLE Goods (
	good_id SERIAL PRIMARY KEY,
	good_name VARCHAR(255),
	type INT
);
```

```sql
INSERT INTO Goods (good_name, type) VALUES ('Table', 8);
```

## Test yourself

The `Goods` table already has a product with `good_id = 3`, and `good_id` is the primary key. What happens when you run this query?

```sql
INSERT INTO Goods (good_id, good_name, type)
VALUES (3, 'chair', 8);
```

1. The table will have two rows with good_id = 3 — The primary key is unique: a table cannot have two rows with the same key value.

2. The row with good_id = 3 will be replaced with the new one — A plain INSERT only adds rows and does not change existing ones. To change records, use the UPDATE statement.

3. **Correct answer:** The query will fail with an error — The primary key value 3 is already taken, so the DBMS refuses to add the row and reports a duplicate key.
