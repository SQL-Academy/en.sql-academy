# Aggregate Functions

In the article about grouping, we discussed that when using the `GROUP BY` operator, we can use aggregate functions. Let's talk more about them 🐳.

> An aggregate function is a function that performs a calculation on a set of values and returns a single value.

## General structure of a query with an aggregate function

```sql
SELECT [literals, aggregate_functions, grouping_fields]
FROM table_name
GROUP BY grouping_fields;
```

For example, a query using the `AVG` aggregate function might look like this:

```sql
SELECT home_type, AVG(price) as avg_price FROM Rooms
GROUP BY home_type
```

## Description of aggregate functions

| Function             | Description                   |
| :------------------- | :---------------------------- |
| `SUM(table_field)`   | Returns the sum of the values |
| `AVG(table_field)`   | Returns the average value     |
| `COUNT(table_field)` | Returns the number of records |
| `MIN(table_field)`   | Returns the minimum value     |
| `MAX(table_field)`   | Returns the maximum value     |

> Aggregate functions apply to values that are not `NULL`. The exception is the `COUNT(*)` function..

## Examples

<ERD databaseName="Airbnb" />

- Find the number of each type of home and sort the resulting list in descending order:

  ```sql
  SELECT home_type, COUNT(*) as amount FROM Rooms
  GROUP BY home_type
  ORDER BY amount DESC
  ```

- For each room, find the latest end date of reservations(the `end_date` field)

  ```sql
  SELECT room_id, MAX(end_date) AS last_end_date FROM Reservations
  GROUP BY room_id
  ```
