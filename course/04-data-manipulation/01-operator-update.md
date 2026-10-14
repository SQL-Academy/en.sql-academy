---
meta:
    title: "Updating data, UPDATE operator"
    description: "How to change data in an SQL table with UPDATE: SET and WHERE syntax, updating several columns in one query, updating based on another table, common mistakes. Examples for MySQL and PostgreSQL."
---

# Updating data, UPDATE operator

Data that has been added does not always stay correct: people change their surnames, typos turn up in payments, prices need to be recalculated. To fix a row without deleting it, use the `UPDATE` operator.

## General query structure with the UPDATE operator

```sql
UPDATE table_name
SET table_field = value [, ...]
[WHERE condition];
```

- `table_name` — the table whose records are changed.
- `table_field` — the column that gets a new value. You can list several `table_field = value` pairs
  separated by commas.
- `value` — the new value: a number, a string, an expression like `price * 1.1`, or `NULL`.
- `condition` (optional) — which rows to change. Without `WHERE`, every row of the table is changed.

For example, Andie (Andie Quincey) has changed his surname and is now Andie Anthony.

Family database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/operator-update).

```sql
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```

| member_id | status   | member_name                     | birthday                 |
| --------- | -------- | ------------------------------- | ------------------------ |
| 1         | father   | Headley Quincey                 | 1960-05-13T00:00:00.000Z |
| 2         | mother   | Flavia Quincey                  | 1963-02-16T00:00:00.000Z |
| 3         | son      | ~~Andie Quincey~~ Andie Anthony | 1983-06-05T00:00:00.000Z |
| 4         | daughter | Lela Quincey                    | 1985-06-07T00:00:00.000Z |
| 5         | daughter | Annie Quincey                   | 1988-04-10T00:00:00.000Z |
| 6         | father   | Ernest Forrest                  | 1961-09-11T00:00:00.000Z |
| 7         | mother   | Constance Forrest               | 1968-09-06T00:00:00.000Z |
| 8         | daughter | Wednesday Addams                | 2005-01-13T00:00:00.000Z |

> Be careful when updating data. If you skip the `WHERE` operator, all entries will be updated.

## Changing several columns

To change several columns at once, list the `table_field = value` pairs after `SET`, separated by commas.
For example, Andie bought 4 cartons of milk at 25 each, but the payment with ID 3 was recorded by mistake as 5 at 20. Let's fix both columns in one query:

```sql
UPDATE Payments
SET amount = 4, unit_price = 25
WHERE payment_id = 3;
```

| payment_id | date                     | family_member | good | amount  | unit_price |
| ---------- | ------------------------ | ------------- | ---- | ------- | ---------- |
| 1          | 2005-02-12T00:00:00.000Z | 1             | 1    | 1       | 2000       |
| 2          | 2005-03-23T00:00:00.000Z | 2             | 1    | 1       | 2100       |
| 3          | 2005-05-14T00:00:00.000Z | 3             | 4    | ~~5~~ 4 | ~~20~~ 25  |
| 4          | 2005-07-22T00:00:00.000Z | 4             | 5    | 1       | 350        |
| 5          | 2005-07-26T00:00:00.000Z | 4             | 7    | 2       | 150        |
| 6          | 2005-02-20T00:00:00.000Z | 5             | 6    | 1       | 100        |
| 7          | 2005-07-30T00:00:00.000Z | 2             | 6    | 1       | 120        |
| 8          | 2005-09-12T00:00:00.000Z | 2             | 16   | 1       | 5500       |
| 9          | 2005-09-30T00:00:00.000Z | 5             | 15   | 1       | 230        |
| 10         | 2005-10-27T00:00:00.000Z | 5             | 15   | 1       | 230        |
| 11         | 2005-11-28T00:00:00.000Z | 5             | 15   | 1       | 250        |
| 12         | 2005-12-22T00:00:00.000Z | 5             | 15   | 1       | 250        |
| 13         | 2005-08-11T00:00:00.000Z | 3             | 13   | 1       | 2200       |
| 14         | 2005-10-23T00:00:00.000Z | 2             | 14   | 1       | 66000      |
| 15         | 2005-02-03T00:00:00.000Z | 1             | 9    | 5       | 8          |
| 16         | 2005-03-11T00:00:00.000Z | 1             | 9    | 5       | 7          |
| 17         | 2005-03-18T00:00:00.000Z | 2             | 9    | 3       | 8          |
| 18         | 2005-04-20T00:00:00.000Z | 1             | 9    | 8       | 8          |
| 19         | 2005-05-13T00:00:00.000Z | 1             | 9    | 5       | 7          |
| 20         | 2005-06-11T00:00:00.000Z | 2             | 9    | 3       | 150        |
| 21         | 2006-01-12T00:00:00.000Z | 3             | 10   | 1       | 100        |
| 22         | 2006-03-12T00:00:00.000Z | 1             | 5    | 3       | 10         |
| 23         | 2005-06-05T00:00:00.000Z | 1             | 8    | 1       | 300        |
| 24         | 2005-06-20T00:00:00.000Z | 3             | 6    | 8       | 150        |
| 25         | 2005-06-21T00:00:00.000Z | 2             | 9    | 3       | 150        |
| 26         | 2005-06-22T00:00:00.000Z | 5             | 18   | 1       | 16         |
| 27         | 2005-06-22T00:00:00.000Z | 3             | 17   | 1       | 59         |
| 28         | 2006-01-15T00:00:00.000Z | 1             | 11   | 1       | 10000      |

All the listed columns change in the same rows — the ones that match the `WHERE` condition.

## Calculated values

A new value can be calculated from the old one — for example, to double all prices:

```sql
UPDATE Payments
SET unit_price = unit_price * 2;
```

The expression can also use other columns of the same row, as long as their types are compatible.
For example, let's record the payment with ID 3 as a single item: `unit_price` will hold the cost of the whole purchase, and `amount` will be one.

```sql
UPDATE Payments
SET unit_price = unit_price * amount,
    amount = 1
WHERE payment_id = 3;
```

**Order of assignments in SET**

**MySQL**

In a single-table `UPDATE`, assignments in `SET` are performed from left to right, and the next expression already sees the new value of the column.
If you swap the assignments, `unit_price` is multiplied by the 1 just written to `amount` and does not change:

```sql
UPDATE Payments
SET amount = 1,
    unit_price = unit_price * amount
WHERE payment_id = 3;
```

So put the assignment to a column that other expressions depend on at the end of the list.

**PostgreSQL**

All expressions in `SET` are calculated from the values the row had before the change, so the order of assignments does not matter.
This query gives the same result:

```sql
UPDATE Payments
SET amount = 1,
    unit_price = unit_price * amount
WHERE payment_id = 3;
```

## Updating based on data from another table

Sometimes the rows to update have to be selected using data from another table. For example, it turned out that every caviar payment was recorded at half the real price.
The simplest way is a subquery in `WHERE`:

```sql
UPDATE Payments
SET unit_price = unit_price * 2
WHERE good IN (
    SELECT good_id
    FROM Goods
    WHERE good_name LIKE '%caviar%'
);
```

**MySQL**

You can do the same by joining the tables with `JOIN` right in the `UPDATE` query:

```sql
UPDATE table_name_1
JOIN table_name_2 ON join_condition
SET table_name_1.table_field = value [, ...]
[WHERE condition];
```

- `table_name_1` — the table whose records are changed.
- `table_name_2` — the joined table whose data helps select the rows and calculate the new values.
- `join_condition` — how the rows are linked, for example `table_name_1.field = table_name_2.field`.
- `condition` (optional) — which rows to change. The condition can check columns of both tables.

```sql
UPDATE Payments
JOIN Goods ON Payments.good = Goods.good_id
SET Payments.unit_price = Payments.unit_price * 2
WHERE Goods.good_name LIKE '%caviar%';
```

**PostgreSQL**

You can do the same by listing the additional tables after `FROM`:

```sql
UPDATE table_name_1
SET table_field = value [, ...]
FROM table_name_2
WHERE join_condition
[AND condition];
```

- `table_name_1` — the table whose records are changed. Do not repeat it after `FROM`.
- `table_field` — a column of `table_name_1`. It is written without the table name.
- `table_name_2` — an additional table whose data helps select the rows and calculate the new values.
- `join_condition` — how the rows are linked, for example `table_name_1.field = table_name_2.field`.
- `condition` (optional) — an extra condition: which of the linked rows to change.

```sql
UPDATE Payments
SET unit_price = unit_price * 2
FROM Goods
WHERE Payments.good = Goods.good_id
    AND Goods.good_name LIKE '%caviar%';
```

**MySQL**

**A subquery on the table being updated**

A subquery cannot read the same table that `UPDATE` changes.
For example, you cannot double the price in payments cheaper than average this way:

```sql
UPDATE Payments
SET unit_price = unit_price * 2
WHERE unit_price < (SELECT AVG(unit_price) FROM Payments);
```

The query fails with an error:

```sql
ERROR 1093 (HY000): You can't specify target table 'Payments' for update in FROM clause
```

You can work around this by wrapping the subquery in one more `SELECT`. The average price is then first calculated and stored in a temporary table, and the update reads from it:

```sql
UPDATE Payments
SET unit_price = unit_price * 2
WHERE unit_price < (
    SELECT avg_price
    FROM (SELECT AVG(unit_price) AS avg_price FROM Payments) AS stats
);
```

## Common mistakes

### Forgotten WHERE

A query without a condition changes every row of the table. Check the condition with a `SELECT` query first, and then put it into `UPDATE`.

### AND instead of a comma in SET

The query below does not change two columns: everything to the right of the first `=` is treated as a single logical expression.

```sql
UPDATE Payments
SET amount = 4 AND unit_price = 25
WHERE payment_id = 3;
```

**MySQL**

The query runs without an error and writes the result of this expression — `0` or `1` — to `amount`, while `unit_price` stays the same.

**PostgreSQL**

The query fails with an error:

```sql
ERROR: argument of AND must be type boolean, not type integer
```

To change both columns, separate the assignments with a comma. That is how `SET` knows where one ends and the next one begins:

```sql
UPDATE Payments
SET amount = 4, unit_price = 25
WHERE payment_id = 3;
```

### Comparing with NULL using =

The condition `WHERE field = NULL` matches no rows, so the query changes nothing. To check for an empty value, use `IS NULL`.

## Test yourself

Some users in the `Users` table have no email confirmation date: their `email_verified_at` field is `NULL`. How many rows will this query change?

```sql
UPDATE Users
SET email_verified_at = '2024-01-01'
WHERE email_verified_at = NULL;
```

1. All rows where email_verified_at is NULL — Comparing with NULL using = never evaluates to true, so the condition does not match even these rows. To find them, write IS NULL.

2. All rows of the table — The query does have a WHERE condition, it just does not hold for any row. Only a query without WHERE would change every row.

3. **Correct answer:** None — The condition email_verified_at = NULL matches no rows, not even those where the value really is empty. Use IS NULL for this check.
