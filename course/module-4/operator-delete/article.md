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

If there is no `WHERE` record selection condition, then all records will be deleted the specified table.

The same operation (deleting all records) can also be done using `TRUNCATE` operator. It will drop the table and re-create it - this option is much faster, than deleting all records one by one (as is the case with `DELETE`) especially for large tables.

## General structure of a query with the TRUNCATE statement

```sql
TRUNCATE TABLE table_name;
```

<MySQLOnly>

> The MySQL query optimizer automatically uses the `TRUNCATE` statement if the `DELETE` statement does not contain a `WHERE` clause or `LIMIT` constructs.

</MySQLOnly>

However, the `TRUNCATE` statement has several differences:

- Triggers are not processed, in particular, the delete trigger
- Deletes all rows in a table without writing the deletion of individual rows of data to the transaction log
- Resets the counter of identifiers to the initial value
- To use, you must have edit rights to the table

## Deleting records for multi-table queries

If the `DELETE` request uses `JOIN`, then it is necessary to specify from which table (s) you want to delete records.

```sql
DELETE table_name_1 [, table_name_2] FROM
table_name_1 JOIN table_name_2
ON table_name_1.field = table_name_2.field
[WHERE the_conditions_of_the_limitations];
```

For example, we need to delete all reservations for a home that does not have a kitchen. Then the request will look like this:

<MySQLOnly>

```sql
DELETE Reservations FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
DELETE FROM Reservations
USING Rooms
WHERE Reservations.room_id = Rooms.id
AND Rooms.has_kitchen = false;
```

</PostgreSQLOnly>

If, in addition to deleting the reservation, we also needed to delete the accommodation, then the query would look like this:

<MySQLOnly>

```sql
DELETE Reservations, Rooms FROM
Reservations JOIN Rooms ON
Reservations.room_id = Rooms.id
WHERE Rooms.has_kitchen = false;
```

</MySQLOnly>

<PostgreSQLOnly>

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

</PostgreSQLOnly>
