# operator HAVING

We have already looked at a query to obtain the average rental cost of residential properties depending on the type of housing:

```sql
SELECT home_type, AVG(price) as avg_price FROM Rooms
GROUP BY home_type
```

| home_type       | avg_price |
| --------------- | --------- |
| Private room    | 89.4286   |
| Entire home/apt | 148.6667  |
| Shared room     | 40        |

Let's modify this query so that only those groups whose average cost is greater than 50 are displayed in the final selection.

With our previous experience, there is a great temptation to try to use the `WHERE` operator for this purpose.
But when attempting to execute such a query, the DBMS will inevitably return an error, indicating that we are using the syntax `WHERE avg_price > 50` incorrectly.

```sql
SELECT home_type, AVG(price) as avg_price FROM Rooms
GROUP BY home_type
WHERE avg_price > 50
```

In advance, to filter groups, we must use the `HAVING` operator:

```sql
SELECT home_type, AVG(price) as avg_price FROM Rooms
GROUP BY home_type
HAVING avg_price > 50
```

| home_type       | avg_price |
| --------------- | --------- |
| Private room    | 89.4286   |
| Entire home/apt | 148.6667  |

<br />

## Order of execution for SQL queries

But why couldn't we use `WHERE`, and why do we need a separate operator for this purpose? It all has to do with the order of execution for SQL queries.

![SQL query execution order scheme](https://sql-academy.org/static/guidePage/operator-having/sql_query_order_en.png "SQL query execution order scheme")

Our first query was incorrect because we tried to use the `avg_price` field for the formed groups before they were formed,
as the execution of the `WHERE` operator precedes grouping.

That is, the `WHERE` operator, at the moment of execution, knows nothing about the subsequent grouping;
it only works with records from the table. With it, for example, we can filter records from the `Rooms` table by price before grouping,
and only then calculate the average cost of the remaining housing groups:

```sql
SELECT home_type, AVG(price) as avg_price FROM Rooms
WHERE price > 50
GROUP BY home_type
```

| home_type       | avg_price |
| --------------- | --------- |
| Private room    | 96.875    |
| Entire home/apt | 148.6667  |

<br />

## General Query Structure with HAVING Operator

```sql
SELECT [constants, aggregate_functions, grouping_fields]
FROM table_name
WHERE row_limit_conditions
GROUP BY grouping_fields
HAVING row_limit_conditions_after_grouping
ORDER BY sorting_condition
```

## Example of Using HAVING

For example, let's get the minimum cost of each type of housing with a TV.
We are only interested in housing types that have at least 5 residential properties associated with them.

To get this result, we must:

- First get all the data from the table

  ```sql
  SELECT ... FROM Rooms;
  ```

- Then select from all the `Room` table entries only those that interest us, i.e. only housing with a TV

  ```sql
  SELECT ... FROM Rooms
  WHERE has_tv = True
  ```

- Then group the data on residential properties by their type

  ```sql
  SELECT ... FROM Rooms
  WHERE has_tv = True
  GROUP BY home_type
  ```

- After that, filter the received groups based on the condition that we are interested in groups that have at least 5 representatives

  ```sql
  SELECT ... FROM Rooms
  WHERE has_tv = True
  GROUP BY home_type
  HAVING COUNT(*) >= 5
  ```

- And finally, to see what is asked of us in the task and accordingly add the output of the necessary information.
  In our case, we need to output the name of the home type and its minimum cost.

  ```sql
  SELECT home_type, MIN(price) as min_price FROM Rooms
  WHERE has_tv = True
  GROUP BY home_type
  HAVING COUNT(*) >= 5;
  ```
