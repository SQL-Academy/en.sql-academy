---
meta:
    title: 'Common Table Expressions, operator WITH'
    description: 'Common Table Expression in SQL. Syntax of the WITH statement and examples of its use.'
---

# Common Table Expressions, operator WITH

Common Table Expressions is a temporary dataset that can be accessed in subsequent queries.
The `WITH` operator is used to write a common table expressions.

```sql-Trip-executable
-- Example of using the WITH clause
WITH Aeroflot_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Aeroflot")

SELECT plane, COUNT(plane) AS amount FROM Aeroflot_trips GROUP BY plane;
```

An expression with `WITH` is considered "temporary" because the result is not stored somewhere permanently
in the database schema, but acts as a temporary representation that exists only for the duration of the query,
that is, it is available only during the execution of a `SELECT`, `INSERT`, `UPDATE`, `DELETE`, or `MERGE` statement.
It is only valid in the query it belongs to, allowing you to improve the structure of the query without polluting the global namespace.

## WITH statement syntax

```sql
WITH name_cte [(column_1 [, column_2 ] …)] AS (subquery)
    [, name_cte [(column_1 [, column_2 ] …)] AS (subquery)] …
```

How to use the `WITH` operator:

1. Initiate the `WITH`
2. Specify the name of common table expression
3. Optional: specify column names separated by commas
4. Enter `AS` operator and subquery, the result of which can be used in other parts of the SQL query, using the name defined in step 2
5. Optional: if more than one table expression is needed, then put a comma and repeat steps 2-4

## Query examples

1. We create a common table expression `Aeroflot_trips` containing all the flights made by the Aeroflot airline

```sql-Trip-executable
WITH Aeroflot_trips AS
    (SELECT plane, town_from, town_to FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Aeroflot")

SELECT * FROM Aeroflot_trips;
```

| plane | town_from | town_to |
| ----- | --------- | ------- |
| IL-86 | Moscow    | Rostov  |
| IL-86 | Rostov    | Moscow  |

2. Similarly, we create a common table expression `Aeroflot_trips`, but with renamed columns

```sql-Trip-executable
WITH Aeroflot_trips (aeroflot_plane, town_from, town_to) AS
    (SELECT plane, town_from, town_to FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Aeroflot")

SELECT * FROM Aeroflot_trips;
```

| aeroflot_plane | town_from | town_to |
| -------------- | --------- | ------- |
| IL-86          | Moscow    | Rostov  |
| IL-86          | Rostov    | Moscow  |

3. Using the `WITH` operator, we define several common table expressions

```sql-Trip-executable
WITH Aeroflot_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Aeroflot"),
    Don_avia_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = "Don_avia")

SELECT * FROM Don_avia_trips UNION SELECT * FROM  Aeroflot_trips;
```

| id   | company | plane  | town_from | town_to | time_out                 | time_in                  |
| ---- | ------- | ------ | --------- | ------- | ------------------------ | ------------------------ |
| 1181 | 1       | TU-134 | Rostov    | Moscow  | 1900-01-01T06:12:00.000Z | 1900-01-01T08:01:00.000Z |
| 1182 | 1       | TU-134 | Moscow    | Rostov  | 1900-01-01T12:35:00.000Z | 1900-01-01T14:30:00.000Z |
| 1187 | 1       | TU-134 | Rostov    | Moscow  | 1900-01-01T15:42:00.000Z | 1900-01-01T17:39:00.000Z |
| 1188 | 1       | TU-134 | Moscow    | Rostov  | 1900-01-01T22:50:00.000Z | 1900-01-02T00:48:00.000Z |
| 1195 | 1       | TU-154 | Rostov    | Moscow  | 1900-01-01T23:30:00.000Z | 1900-01-02T01:11:00.000Z |
| 1196 | 1       | TU-154 | Moscow    | Rostov  | 1900-01-01T04:00:00.000Z | 1900-01-01T05:45:00.000Z |
| 1145 | 2       | IL-86  | Moscow    | Rostov  | 1900-01-01T09:35:00.000Z | 1900-01-01T11:23:00.000Z |
| 1146 | 2       | IL-86  | Rostov    | Moscow  | 1900-01-01T17:55:00.000Z | 1900-01-01T20:01:00.000Z |

## Conclusion

Common table expressions have been added to SQL to simplify complex long queries, especially those with multiple subqueries. Their main task is to improve readability,
ease of writing queries and their further support. It does this by hiding large and complex queries in named expressions, which are then used in the main query.
