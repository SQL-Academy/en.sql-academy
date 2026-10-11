---
meta:
    title: "Syntax of DELETE operator"
    description: "Delete records in sql. SQL operators delete, truncate and their differences. Delete query c join"
---

# Syntax of DELETE operator

From time to time the task of deleting records from the table arises.
For this, SQL provides operators `DELETE` and `TRUNCATE`, of which the first option is the most universal and safe.

## General structure of a query with an operator DELETE

```sql
DELETE FROM table_name
[WHERE condition];
```

- `table_name` — the table from which records are deleted.
- `condition` (optional) — which rows to delete. Without `WHERE`, every row of the table is deleted.

You can also delete all records of a table using the `TRUNCATE` operator. It will drop the table and re-create it - this option is much faster, than deleting all records one by one (as is the case with `DELETE`) especially for large tables.

## General structure of a query with the TRUNCATE statement

```sql
TRUNCATE TABLE table_name;
```

- `table_name` — the table to clear completely.

**MySQL**

> The MySQL query optimizer automatically uses the `TRUNCATE` statement if the `DELETE` statement does not contain a `WHERE` clause or `LIMIT` constructs.

However, the `TRUNCATE` statement has several differences:

**MySQL**

- Triggers are not processed, in particular, the delete trigger
- Deletes all rows in a table without writing the deletion of individual rows of data to the transaction log
- Resets the counter of identifiers to the initial value
- To use, you must have edit rights to the table

**PostgreSQL**

- Triggers are not processed, in particular, the delete trigger
- Deletes all rows in a table without writing the deletion of individual rows of data to the transaction log
- Can reset the identifier counter when using the `RESTART IDENTITY` option (`CONTINUE IDENTITY` is used by default, so the counter is not reset)
- To use, you must have edit rights to the table

## Deleting records for multi-table queries

**MySQL**

If the `DELETE` query uses a `JOIN`, you need to specify which tables should have their rows removed.

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

If the `DELETE` query uses `USING`, specify the additional tables that help select the rows to delete after it.

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

For example, we need to delete all reservations for a home that does not have a kitchen. Then the request will look like this:

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

If, in addition to deleting the reservation, we also needed to delete the accommodation, then the query would look like this:

**MySQL**

```sql
DELETE Reservations, Rooms FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
```

**PostgreSQL**

In PostgreSQL, to delete from multiple tables simultaneously, separate DELETE queries or transactions are used:

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
