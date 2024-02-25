---
meta:
    title: 'Subqueries with multiple rows and one column'
    description: 'Subqueries using ANY, IN, ALL operators'
---

# Subqueries with multiple rows and one column

If a subquery returns more than one row, it cannot be used with comparison operators in the same way as scalar subqueries could be used <a href="https://sql-academy.org/guide/subquery-with-one-column-one-row" target="_blank">as described here</a>.

However, with subqueries that return multiple rows and one column, there are three additional operators that can be used.

## Subquery and ALL operator

With the `ALL` operator, we can compare an individual value to each value in the set obtained by the subquery.
In this case, this condition will return `TRUE` only if all comparisons of an individual value with values in the set return `TRUE`.

For example, the synthetic query below checks whether the condition that a residential unit is cheaper than 200 is true for all residential units.

```sql
SELECT 200 > ALL(SELECT price FROM Rooms)
```

Or a more practical example: we need to find the names of all property owners who have never rented out their property themselves.
To obtain this list, we can proceed as follows:

- Get a list of names of all property owners
  ```sql
  SELECT DISTINCT name FROM Users INNER JOIN Rooms
  ON Users.id = Rooms.owner_id
  ```
- Get a list of identifiers of all users who have rented out property

  ```sql
  SELECT DISTINCT user_id FROM Reservations
  ```

- Filter the first list of all property owners by the condition that the property owner identifier is not equal to any of the identifiers of users who have ever rented out property

  ```sql
  SELECT DISTINCT name FROM Users INNER JOIN Rooms
      ON Users.id = Rooms.owner_id
      WHERE Users.id <> ALL (
          SELECT DISTINCT user_id FROM Reservations
      )
  ```

## Subquery and IN operator

The `IN` operator checks whether a specific value is included in a set of values. Such a set can be a subquery that returns multiple rows with one column.

For example, if we need to obtain all information about property owners whose property costs more than 150 units, we can do it as follows:

```sql
SELECT * FROM Users WHERE id IN (
    SELECT DISTINCT owner_id FROM Rooms WHERE price >= 150
)
```

## Subquery and ANY operator

The conditional expression with `ANY` has similar behavior, but it returns `TRUE` if at least one comparison of an individual value with a value in the set returns `TRUE`.

Let's use it to write the same query that we did with the `IN` operator: find users who own at least one residential unit that costs more than 150.

```sql
SELECT * FROM Users WHERE id = ANY (
    SELECT DISTINCT owner_id FROM Rooms WHERE price >= 150
)
```
