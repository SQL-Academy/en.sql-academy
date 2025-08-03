---
meta:
  title: "Grouping, GROUP BY operator"
  description: "The structure of an SQL query with the group by operator, grouping by multiple fields, and examples."
---

# Grouping, GROUP BY operator

Let's run a query:

```sql-executable-Airbnb
SELECT id, home_type, has_tv, price FROM Rooms;
```

This gives us information about each rented room. But what if we want to get information not about each record separately, but about the groups they form?

For example, such groups can be records divided by the type of housing:

- Shared room (renting a room for several people)
- Private room (renting a whole room)
- Entire home/apt (renting a whole apartment)

These groups include different records in the table and, accordingly, have different characteristics that can be very useful to us.

Useful information about groups can include:

- the average rental cost of a room or entire living space
- the number of rented living spaces of each type

To answer all these and many other questions, there is the `GROUP BY` operator.

## The general structure of a query with GROUP BY

```sql
SELECT [literals, aggregate_functions, grouping_fields]
FROM table_name
GROUP BY grouping_fields;
```

To group records by the type of housing, we need to specify `home_type` after `GROUP BY`, i.e., the field by which grouping will occur.

```sql-executable-Airbnb
SELECT home_type FROM Rooms
GROUP BY home_type
```

> It should be noted that for `GROUP BY`, all `NULL` values are treated as equal,
> i.e., when grouping by a field that contains `NULL` values, all such rows will be included in one group.

When using the `GROUP BY` operator, we have moved from working with individual records to working with formed groups.
Because of this, we cannot simply output any field from a record (such as `has_tv` or `price`), as we could before.
This is because there may be several records in each group, and each of them may have a different value in this field.

When using `GROUP BY`, we can only output:

- literals, i.e., values that are explicitly fixed.

  We can output them because they are fixed values that do not depend on anything.
  For example,

  ```sql-executable-Airbnb
  SELECT home_type, 'literal' FROM Rooms
  GROUP BY home_type
  ```

- aggregate function results, i.e. computed values based on a set of values.

  We will cover more detailed information about aggregate functions in the next lesson. But for example, let's consider the aggregate function `AVG`.
  The `AVG` function takes as an argument the name of the field we want to calculate the average value for each group based on.

  ```sql-executable-Airbnb
  SELECT home_type, AVG(price) as avg_price FROM Rooms
  GROUP BY home_type
  ```

  This query first divides all records from the `Rooms` table into 3 groups based on the `home_type` field.
  Then, for each group, it adds up all the values taken from the `price` field of each record included in the current group, and then divides the resulting sum
  by the number of records in that group.

- grouping fields.

  We can output them because within one group, the fields on which grouping was performed are the same.

## Grouping by 2 or more fields

    We have already looked at how records in a table are grouped by one field. For additional illustration,
    it looks something like this when the grouping field is `home_type`:

    ![Grouping by 1 field](https://sql-academy.org/static/guidePage/groupping/groupping_by_1_field.png 'Grouping by 1 field')

    When grouping by 2 or more fields, the principle remains the same,
    only the resulting groups are additionally divided into smaller groups depending on the second grouping field.

    Example of grouping by `home_type` and `has_tv`:

    ![Grouping by 2 field](https://sql-academy.org/static/guidePage/groupping/groupping_by_2_field.png 'Grouping by 2 field')

Let's test ourselves? When using the `GROUP BY` operator, what can display in `SELECT` statement?
