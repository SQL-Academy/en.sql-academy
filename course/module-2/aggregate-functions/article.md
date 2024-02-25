---
meta:
    title: 'Aggregate Functions'
    description: 'Aggregate functions in SQL queries, syntax, and examples'
---

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

  | home_type       | amount |
  | --------------- | ------ |
  | Private room    | 28     |
  | Entire home/apt | 21     |
  | Shared room     | 1      |

- For each room, find the latest end date of reservations(the `end_date` field)

  ```sql
  SELECT room_id, MAX(end_date) AS last_end_date FROM Reservations
  GROUP BY room_id
  ```

  | room_id | last_end_date        |
  | ------- | -------------------- |
  | 1       | 2019-02-04T12:00:00Z |
  | 2       | 2020-03-23T09:00:00Z |
  | 13      | 2020-04-21T10:00:00Z |
  | 16      | 2019-06-24T10:00:00Z |
  | 21      | 2020-02-29T10:00:00Z |
  | 19      | 2020-05-02T10:00:00Z |
  | 8       | 2020-01-21T12:00:00Z |
  | 7       | 2019-09-17T10:00:00Z |
  | 5       | 2020-05-15T10:00:00Z |
  | 50      | 2019-11-25T11:00:00Z |
  | 49      | 2020-06-11T10:00:00Z |
  | 48      | 2019-11-10T10:00:00Z |
  | 32      | 2020-01-18T13:00:00Z |
  | 17      | 2019-11-05T09:00:00Z |
  | 25      | 2020-04-22T09:00:00Z |
  | 14      | 2020-02-12T10:00:00Z |
  | 39      | 2019-12-09T10:00:00Z |
  | 38      | 2020-03-23T10:00:00Z |
