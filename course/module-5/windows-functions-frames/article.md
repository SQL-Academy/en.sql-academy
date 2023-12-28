# Window frames

In the context of SQL window functions, a "window" defines a subset of rows
that are considered by the SQL function when performing calculations.

In other words, a window is a dynamic set of rows that "slides" through your query result,
forming different data sets for each row, depending on your defined window.

## Window vs Partition

Although the terms "window" and "partition" may seem similar, they represent different concepts:

- Partition (`PARTITION BY`). This is the division of the entire result set into non-overlapping subsets,
  where each subset contains rows with the same values in one or more columns.
  Window functions are applied separately to each partition, as if each were a separate data set.

  ![Partition divition schema](https://sql-academy.org/static/guidePage/windows-functions-frames/partitions_visualisation_en.png "Partition divition schema")

- Window. Defines which specific rows in each partition will be used
  for calculating the window function for each row.
  The window can change from row to row.

  For example, if the rule `ROWS BETWEEN 1 PRECEDING AND CURRENT ROW` is used,
  for each row the window will consist of the row itself and one preceding row.
  This is like a "subpartition" within an existing partition.

  ![Partition divition schema](https://sql-academy.org/static/guidePage/windows-functions-frames/windows_visualisation_en.png "Partition divition schema")

  That is:

  - The first window consists only of the 1st record, because there is no previous record.
    The single record is passed to the aggregate function `AVG(price)` and the result is added to the `avg_price` field.
  - The second window already contains records 1 and 2, which are sent to `AVG(price)` and return `(170 + 220) / 2 = 195`.
  - The third window contains records 2 and 3, resulting in `(220 + 150) / 2 = 185`.
  - and so on.

### Note on window without ROWS/RANGE

If `ROWS/RANGE` is missing in the definition of a window function,
then by default the window coincides with the partition.
In this case, the window function will process all rows within the partition, not limited to a subset.
This means that the function result will be the same for all rows within the same partition.

## Defining window frames

Using the `ROWS` or `RANGE` syntax, we can define exactly which window of data will be passed to the window function
for calculating the value for the current row.

The syntax for defining window frames looks like specifying a range relative to the current row.

```sql
SELECT  <window_function>(<table_field>)
OVER (
      ...
      ROWS|RANGE BETWEEN <start of window frame> AND <<end of window frame>
)
```

For example, if we want only the two preceding records and the current row to be included in the window function calculation, then
the syntax would look like this:

```sql
... ROWS|RANGE BETWEEN 2 PRECEDING AND CURRENT ROW
```

If we want the window function to include the current row and all subsequent ones, then the syntax would look
like this:

```sql
... ROWS|RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
```

### Possible window frames definitions

- `UNBOUNDED PRECEDING`, all rows preceding the current one
- `N PRECEDING`, N rows before the current row
- `CURRENT ROW`, the current row
- `N FOLLOWING`, N rows after the current row
- `UNBOUNDED FOLLOWING`, all subsequent rows

### Window frame definition schema

![Window frame definition schema](https://sql-academy.org/static/guidePage/windows-functions-frames/window-definition.png "Window frame definition schema")

## The difference between ROWS and RANGE

For defining window frames, there are the keywords `ROWS` and `RANGE`. They work differently:

### ROWS

- Based on physical rows:

  When using `ROWS`, the window definition is based on the physical position of rows relative to the current row.
  For example, `1 PRECEDING `means one row before the current one.

- Precise frame:

  Defining a window with `ROWS` clearly limits the number of rows included in the window,
  making it predictable and specific.

![Window frame definition schema with rows](https://sql-academy.org/static/guidePage/windows-functions-frames/rows_example_en.png "Window frame definition schema with rows")

### RANGE

- Based on values:

  `RANGE`, unlike `ROWS`, defines window frames based on column values,
  ordered according to `ORDER BY` in the window function.

- Dynamic frames:

  Frames defined with `RANGE` can vary
  depending on the data, making the window flexible but potentially less predictable.

![Window frame definition schema with range](https://sql-academy.org/static/guidePage/windows-functions-frames/range_example_en.png "Window frame definition schema with range")
