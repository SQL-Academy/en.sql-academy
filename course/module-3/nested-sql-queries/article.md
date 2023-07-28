# Subqueries

Subqueries are one of the most powerful 💪 tools in SQL, which can be used in any type of query.
In the upcoming lessons, we will learn about the main types of subqueries and see examples of how they can be used.

> A subquery is a query used in another SQL query.
> The subquery is always enclosed in parentheses and usually executed before
> the main query

Like any other SQL query, a subquery returns a result set, which can be one of the following:

- one row and one column;
- multiple rows with one column;
- multiple rows with multiple columns.

Depending on the type of the subquery result set, the operators that can be used in the main query are determined.

## Example

Let's get a list of all bookings for the most expensive room at the moment:

```sql
SELECT * FROM Reservations
    WHERE Reservations.room_id = (
        SELECT id FROM Rooms ORDER BY price DESC LIMIT 1
    )
```

In this case, the query to get the most expensive room is executed as a subquery,
and then the result of the result set is applied in the main query.

```sql
SELECT id FROM Rooms ORDER BY price DESC LIMIT 1
```

| id  |
| --- |
| 21  |

We will learn more about each type of subquery in the following lessons.
