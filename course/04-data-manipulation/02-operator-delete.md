---
meta:
    title: "Deleting data, DELETE operator"
    description: "How to delete records from an SQL table with DELETE: syntax with WHERE, deleting based on another table with a subquery and JOIN, differences between DELETE and TRUNCATE. Examples for MySQL and PostgreSQL."
---

# Deleting data, DELETE operator

When data is no longer needed, it gets deleted: a product is removed from the list, a cancelled reservation is erased. Rows are deleted with the `DELETE` operator, and a whole table can also be cleared with `TRUNCATE`. Note that emptying a table with `TRUNCATE` cannot always be undone.

## General query structure with the DELETE operator

```sql
DELETE FROM table_name
[WHERE condition];
```

- `table_name` — the table from which records are deleted.
- `condition` (optional) — which rows to delete. Without `WHERE`, every row of the table is deleted.

For example, there is not a single payment for the vacuum cleaner in the `Payments` table, so it can safely be removed from the `Goods` list:

Family database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/operator-delete).

```sql
DELETE FROM Goods
WHERE good_name = 'vacuum cleaner';
```

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
| ~~12~~  | ~~vacuum cleaner~~   | ~~8~~ |
| 13      | jacket               | 7     |
| 14      | fur coat             | 7     |
| 15      | music school fee     | 6     |
| 16      | english school fee   | 6     |
| 17      | airsoft game session | 4     |
| 18      | netflix subscription | 4     |

The `WHERE` condition is written the same way as in a `SELECT` query: you can compare values, combine conditions with `AND` and `OR`, and use `IN`, `LIKE`, and subqueries.

> Be careful when deleting data. If you skip the `WHERE` operator, all entries in the table will be deleted.
> Before running `DELETE`, it helps to run a `SELECT` with the same `WHERE` condition and make sure it returns exactly the rows you are going to delete.

Try it yourself: choose a condition, see which rows would be deleted, and then run the query.

The interactive demonstration is available [in the SQL Academy lesson](https://sql-academy.org/en/guide/operator-delete).

A query without a condition empties the whole table, while the table itself remains:

```sql
DELETE FROM Payments;
```

You can also delete all records of a table using the `TRUNCATE` operator.
It does not go through the rows one by one like `DELETE`, but empties the whole table at once, so it is much faster, especially on large tables.

## General structure of a query with the TRUNCATE operator

```sql
TRUNCATE TABLE table_name;
```

- `table_name` — the table to clear completely.

At the same time, `TRUNCATE` behaves differently from `DELETE`:

**MySQL**

- You cannot specify a `WHERE` condition — the whole table is emptied
- Delete triggers do not fire
- The action cannot be undone: `TRUNCATE` immediately commits the current [transaction](https://sql-academy.org/en/guide/transactions), and `ROLLBACK` does not bring the rows back
- Resets the `AUTO_INCREMENT` counter to the initial value
- Does not work if the table is referenced by foreign keys of other tables
- To use it, you need the `DROP` privilege on the table

**PostgreSQL**

- You cannot specify a `WHERE` condition — the whole table is emptied
- Row delete triggers are not processed — only `TRUNCATE` triggers fire, if any are defined
- The action can be undone: inside a [transaction](https://sql-academy.org/en/guide/transactions), `ROLLBACK` brings the deleted rows back
- Can reset the identifier counter when using the `RESTART IDENTITY` option (`CONTINUE IDENTITY` is used by default, so the counter is not reset)
- If the table is referenced by foreign keys of other tables, you need to add the `CASCADE` option — then those tables are emptied as well
- To use it, you need the `TRUNCATE` privilege on the table

To learn more about how `DELETE` differs from `TRUNCATE` and `DROP`, read the article [TRUNCATE vs DELETE vs DROP in SQL: The Difference and What to Choose](https://sql-academy.org/en/blog/sql-truncate-delete-drop).

## Deleting based on data from another table

Sometimes the rows to delete have to be selected using data from another table.
Suppose the service no longer works with homes that have no kitchen, and the reservations for such homes have to be deleted. Information about the kitchen is stored in the `Rooms` table, and reservations are stored in the `Reservations` table.

Airbnb database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/operator-delete).

The simplest way is a subquery in `WHERE` that returns the IDs of the matching homes:

```sql
DELETE FROM Reservations
WHERE room_id IN (
    SELECT id
    FROM Rooms
    WHERE has_kitchen = false
);
```

**MySQL**

You can do the same with `JOIN`. In this case, list the tables you delete records from right after `DELETE`.

```sql
DELETE table_name_1 [, table_name_2] FROM
table_name_1 JOIN table_name_2
ON join_condition
[WHERE condition];
```

- `table_name_1` — the table from which records are deleted. Its name goes both after `DELETE` and in the
  `JOIN`.
- `table_name_2` — the joined table whose data helps select the records to delete. If you also list it
  after `DELETE`, records are deleted from both tables.
- `join_condition` — how the rows are linked, for example `table_name_1.field = table_name_2.field`.
- `condition` (optional) — which records to delete. The condition can check columns of both tables.

**PostgreSQL**

You can do the same with `USING`. After it, specify the additional tables that help select the rows to delete.

```sql
DELETE FROM table_name_1
USING table_name_2
WHERE join_condition
[AND condition];
```

- `table_name_1` — the table from which records are deleted.
- `table_name_2` — an additional table whose data helps select the records to delete. Nothing is deleted
  from it.
- `join_condition` — how the rows are linked, for example `table_name_1.field = table_name_2.field`.
- `condition` (optional) — an extra condition: which of the linked records to delete. It can check columns of
  both tables.

For our task, the query looks like this:

**MySQL**

```sql
DELETE Reservations FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
```

**PostgreSQL**

```sql
DELETE FROM Reservations
USING Rooms
WHERE Reservations.room_id = Rooms.id
AND Rooms.has_kitchen = false;
```

Now let's delete the homes themselves along with the reservations.

**MySQL**

```sql
DELETE Reservations, Rooms FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
```

This query deletes only the homes that had reservations: `Rooms` rows without a match in `Reservations` do not get into the join.

**PostgreSQL**

The `DELETE` operator removes rows from only one table, so let's delete them one by one: first the bookings, then the housing itself.
To make them run together or not at all, combine them into a [transaction](https://sql-academy.org/en/guide/transactions):

```sql
BEGIN;
DELETE FROM Reservations
USING Rooms
WHERE Reservations.room_id = Rooms.id
AND Rooms.has_kitchen = false;

DELETE FROM Rooms
WHERE Rooms.has_kitchen = false;
COMMIT;
```

The second query deletes all housing without a kitchen, including housing that has never been booked.

**MySQL**

**A subquery on the table you delete from**

A subquery cannot read the same table that `DELETE` removes rows from.
For example, you cannot delete reservations cheaper than the average price this way:

```sql
DELETE FROM Reservations
WHERE price < (SELECT AVG(price) FROM Reservations);
```

The query fails with an error:

```sql
ERROR 1093 (HY000): You can't specify target table 'Reservations' for update in FROM clause
```

You can work around this by wrapping the subquery in one more `SELECT`. The average price is then first calculated and stored in a temporary table, and the deletion reads from it:

```sql
DELETE FROM Reservations
WHERE price < (
    SELECT avg_price
    FROM (SELECT AVG(price) AS avg_price FROM Reservations) AS stats
);
```

## Test yourself

You need to delete only the reservations cheaper than 100 from the `Reservations` table. Which query does this?

1. TRUNCATE TABLE Reservations WHERE price \< 100; — TRUNCATE does not accept a condition and always empties the whole table, so this query fails with an error.

2. **Correct answer:** DELETE FROM Reservations WHERE price \< 100; — DELETE with a WHERE condition removes only the reservations with a price below 100, and the rest stay in the table.

3. DELETE FROM Reservations; — Without a WHERE condition, the query deletes every reservation, not just the cheap ones.
