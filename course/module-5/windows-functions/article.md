---
meta:
    title: 'SQL Window Functions'
    description: 'SQL window functions, OVER syntax of a data window, window function, example of using a window function, execution queue of window functions in a select query'
---

# SQL Window Functions

Window functions are a powerful tool in SQL that allow you to perform complex calculations on groups of rows that are related to the current row.

## How they work

You might be wondering, what does "window" mean? In a standard SQL query, all sets of rows are treated
as one continuous block of data, and aggregate values are calculated for that block.
However, when window functions are applied, the query is segmented into groups of rows, or "windows,"
and individual aggregate values are calculated for each segment. The window that is passed
to the window function can be:

- the entire table,
- separate partitions of the table, which are groups of rows based on one or more fields,
- or even a specific range of rows within the table or partition.

For example, you can define a window that consists of the previous row plus the current row of the table.
In this case, the aggregate function value will be calculated differently for each row, as the data passed to the function dynamically changes from row to row. The window "slides" through the table.

### Visualization

Window functions always take a window of data as input, specified by the user,
and return the result in a separate column. Let's consider an example using the `AVG` window function
to calculate the average value. Here's a small table:

![Partitioning schema](https://sql-academy.org/static/guidePage/windows-functions/1_en.png "Partitioning schema")

Now let's see how the window function works for different windows:

- If the entire table is specified as the window, the window will be the same for all rows,
  and the same set of data will be passed to the `AVG` function, resulting in the same result.

  ![Partitioning schema](https://sql-academy.org/static/guidePage/windows-functions/2_en.png "Partitioning schema")

- If a partition is specified based on the `home_type` field, the `AVG` function
  will receive a set of residential properties with the same type,
  and the result will show the average cost of housing for the type that matches the current row.

  ![Partitioning schema](https://sql-academy.org/static/guidePage/windows-functions/3_en.png "Partitioning schema")

- A more specific set of rows can also be specified as the window. For example,
  the window can be defined as the "previous row + current row" of the table.
  In this case, it would look like this:

  ![Partitioning schema](https://sql-academy.org/static/guidePage/windows-functions/4_en.png "Partitioning schema")

  It's worth noting that for the first row, the window will consist of only one record, as there is no previous row.

## Syntax of window functions

```sql
SELECT <window_function>(<table_field>)
OVER (
      [PARTITION BY <partition_columns>]
      [ORDER BY <sort_columns>]
      [ROWS|RANGE <range_definition>]
)
```

Where:

- `<window_function>(<table_field>)` is the window function being used, e.g., `AVG(price)`.
- `OVER` is used to define the window (group of rows) that will be passed to the window function.
  If `OVER()` is left without parameters, the window will be the entire table.

Within `OVER`, there are three optional parameters that allow you to customize the window:

- `PARTITION BY <partition_columns>` divides the data into non-overlapping subsets, where each subset contains rows with the same values in one or more columns, creating partitions.
- `ORDER BY <sort_columns>` sets the order of the rows within the window. This is particularly important for ranking window functions.
- `ROWS|RANGE <range_definition>` defines the range of rows. This parameter allows you to specify how many rows to include before and after the current row in the window.

We will delve into each of these parameters in more detail in the following articles.

## Example of using window functions

Let's use window functions to get a list of student names and the number of students studying with them in the same class.

<ERD databaseName="Schedule" />

First, let's retrieve a list of students and their corresponding class IDs:

```sql-Schedule-executable
SELECT
    Student.first_name,
    Student.last_name,
    Student_in_class.class
FROM
    Student_in_class
JOIN
    Student ON Student_in_class.student = Student.id;
```

To calculate the number of students studying in each class and display this information in a new column, we can use a window function:

```sql-Schedule-executable
SELECT
    Student.first_name,
    Student.last_name,
    Student_in_class.class,
    COUNT(*) OVER (PARTITION BY Student_in_class.class) AS student_count_in_class
FROM
    Student_in_class
JOIN
    Student ON Student_in_class.student = Student.id;
```

### What does our window function do?

The expression `PARTITION BY Student_in_class.class` divides all rows of the table into
partitions based on the `class` field. This means that for each row, only the rows where the `class`
field matches the `class` field of the current row will be passed to the window function.

The `COUNT` function returns the number of rows passed to it, giving us the number of students studying in each class.

## Execution order of window functions in SELECT

When using window functions, it is important to understand the order in which they are executed. As shown in the diagram below, windows are processed as the penultimate step, after filtering and grouping, but before the final sorting of the query results.

![Execution order of window functions in SELECT query](https://sql-academy.org/static/guidePage/windows-functions/query-order_en.png "Execution order of window functions in SELECT query")

## Conclusion

In this article, we briefly covered the concept of window functions, their capabilities, and practical benefits. In the following articles, we will delve into each aspect of window functions in more detail.
