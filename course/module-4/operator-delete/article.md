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
[WHERE the_conditions_of_the_limitations];
```

If there is no `WHERE` record selection condition, then all records in the specified table will be deleted.

The same operation (deleting all records) can also be done using `TRUNCATE` operator. It will drop the table and re-create it - this option is much faster, than deleting all records one by one (as is the case with `DELETE`) especially for large tables.

## General structure of a query with the TRUNCATE statement

```sql
TRUNCATE TABLE table_name;
```

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
DELETE table_name_1 FROM
table_name_1 JOIN table_name_2
ON table_name_1.field = table_name_2.field
[WHERE the_conditions_of_the_limitations];
```

**PostgreSQL**

If the `DELETE` query uses `USING`, specify the additional tables that help select the rows to delete after it.

```sql
DELETE FROM table_name_1
USING table_name_2
WHERE table_name_1.field = table_name_2.field
[AND the_conditions_of_the_limitations];
```

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
