---
meta:
  title: "Common Table Expressions, operator WITH"
  description: "Common Table Expression in SQL. Syntax of the WITH statement and examples of its use."
---

# Common Table Expressions, operator WITH

Common Table Expressions is a temporary dataset that can be accessed in subsequent queries.
The `WITH` operator is used to write a common table expressions.

```sql-Trip-executable
-- Example of using the WITH clause
WITH Aeroflot_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = 'Aeroflot')

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
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = 'Aeroflot')

SELECT * FROM Aeroflot_trips;
```

2. Similarly, we create a common table expression `Aeroflot_trips`, but with renamed columns

```sql-Trip-executable
WITH Aeroflot_trips (aeroflot_plane, town_from, town_to) AS
    (SELECT plane, town_from, town_to FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = 'Aeroflot')

SELECT * FROM Aeroflot_trips;
```

3. Using the `WITH` operator, we define several common table expressions

```sql-Trip-executable
WITH Aeroflot_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = 'Aeroflot'),
    Don_avia_trips AS
    (SELECT TRIP.* FROM Company
        INNER JOIN Trip ON Trip.company = Company.id WHERE name = 'Don_avia')

SELECT * FROM Don_avia_trips UNION SELECT * FROM  Aeroflot_trips;
```

## Working with recursion in CTE

CTEs can also be used to perform recursive queries, which allow iterative data processing, for example, working with hierarchical data structures such as manager-subordinate relationships.

### Syntax of a recursive CTE

A recursive CTE consists of two parts separated by the `UNION ALL` operator:

- The initial dataset, which does not contain recursive references.
- The recursive part: a query that refers to the CTE to continue the recursion.

```sql
WITH RECURSIVE cte_name (column_1, column_2, ...) AS (
    -- Initial dataset
    SELECT column_1, column_2, ...
    FROM table
    WHERE condition

    UNION ALL

    -- Recursive part
    SELECT column_1, column_2, ...
    FROM cte_name
    INNER JOIN table ON cte_name.column = table.column
    WHERE condition
)

SELECT * FROM cte_name;
```

### Example: manager-subordinate hierarchy

Consider the `Employees` table, which contains employee IDs and their managers:

We need to find all subordinates of John Smith (`id=1`) at all hierarchy levels.

```sql
WITH RECURSIVE Subordinates AS (
    -- Initial dataset
    SELECT id, name, managerId
    FROM Employees
    WHERE managerId = 1

    UNION ALL

    -- Recursive part: subordinates of subordinates
    SELECT e.id, e.name, e.managerId
    FROM Employees e
    INNER JOIN Subordinates s ON e.managerId = s.id
)

SELECT * FROM Subordinates;
```

### Steps for executing a recursive CTE

1. **Initial dataset:** selects all employees whose `managerId=1` (direct subordinates of `John Smith`).
2. **Recursive part:** for each employee selected in the initial dataset, selects their subordinates (where `managerId` equals the `id` of the chosen employee).
3. **Union:** рunites the results of the initial dataset and the recursive parts using `UNION ALL`.
4. **Recursion:** the process repeats for each new set of subordinates until all hierarchy levels are retrieved.

## Conclusion

Common table expressions have been added to SQL to simplify complex long queries, especially those with multiple subqueries. Their main task is to improve readability,
ease of writing queries and their further support. It does this by hiding large and complex queries in named expressions, which are then used in the main query.
