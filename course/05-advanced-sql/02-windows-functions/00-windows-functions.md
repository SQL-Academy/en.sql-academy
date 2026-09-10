---
meta:
    title: "SQL Window Functions"
    description: "SQL window functions, OVER syntax of a data window, window function, example of using a window function, execution queue of window functions in a select query"
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

![Initial table](https://sql-academy.org/static/guidePage/windows-functions/schema_table_en.png "Initial table")

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

Let's use window functions to get a list of students and their class sizes.

Schedule database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/windows-functions).

First, let's retrieve a list of students and their corresponding class IDs:

```sql
SELECT
    Student.first_name,
    Student.last_name,
    Student_in_class.class
FROM
    Student_in_class
JOIN
    Student ON Student_in_class.student = Student.id;
```

| first_name | last_name    | class |
| ---------- | ------------ | ----- |
| Nikolaj    | Sokolov      | 9     |
| Vyacheslav | Eliseev      | 9     |
| Ivan       | Efremov      | 9     |
| Anatolij   | ZHdanov      | 9     |
| Georgij    | Noskov       | 9     |
| Artyom     | Sergeev      | 9     |
| Arina      | Evseeva      | 9     |
| Angelina   | Voroncova    | 9     |
| Ekaterina  | Ustinova     | 9     |
| Raisa      | Lapina       | 9     |
| Leonid     | Ignatov      | 9     |
| Snezhana   | Seliverstova | 9     |
| Semyon     | Biryukov     | 9     |
| Georgij    | Baranov      | 8     |
| YUliya     | Vishnyakova  | 8     |
| Valentina  | Bolshakova   | 8     |
| Leonid     | Kryukov      | 8     |
| Vladislav  | Cvetkov      | 8     |
| Snezhana   | Morozova     | 8     |
| Lyubov     | Borisova     | 8     |
| Anfisa     | Kalashnikova | 8     |
| Anna       | Osipova      | 8     |
| Kristina   | Myasnikova   | 8     |
| Kristina   | Smirnova     | 8     |
| Boris      | Simonov      | 7     |
| Dmitrij    | Trofimov     | 7     |
| YAkov      | Rozhkov      | 7     |
| Fyodor     | Drozdov      | 7     |
| Gleb       | Strelkov     | 7     |
| Angelina   | Lukina       | 7     |
| Nina       | Odincova     | 7     |
| Valeriya   | Novikova     | 7     |
| Grigorij   | Kapustin     | 7     |
| Vitalij    | Panfilov     | 7     |
| Svyatoslav | Tarasov      | 6     |
| Matvej     | YAkushev     | 6     |
| Ilya       | Alekseev     | 6     |
| Lyubov     | Zaharova     | 6     |
| Polina     | Sidorova     | 6     |
| Elizaveta  | Samojlova    | 6     |
| YUliya     | Avdeeva      | 6     |
| Matvej     | Bogdanov     | 6     |
| Ilya       | Filippov     | 6     |
| Denis      | Mel          | 6     |
| Svyatoslav | Muravyov     | 6     |
| Anna       | Kulagina     | 5     |
| ZHanna     | Fokina       | 5     |
| Valeriya   | Lapina       | 5     |
| Valentina  | Sazonova     | 5     |
| Nataliya   | Myasnikova   | 5     |
| Viktoriya  | Makarova     | 5     |
| Stanislav  | Lazarev      | 5     |
| Gennadij   | Ovchinnikov  | 5     |
| Roman      | SHilov       | 4     |
| Timur      | Subbotin     | 4     |
| Danila     | Osipov       | 4     |
| Arina      | Silina       | 4     |
| Nadezhda   | Zaharova     | 4     |
| Larisa     | SHCHerbakova | 4     |
| Aleksandra | Belozyorova  | 4     |
| Natalya    | Davydova     | 4     |
| Mariya     | Fadeeva      | 4     |
| YUrij      | Markov       | 3     |
| Kirill     | SHubin       | 3     |
| Grigorij   | Kolobov      | 3     |
| Semyon     | Trofimov     | 3     |
| Vasilij    | Ustinov      | 3     |
| Valentina  | SHarova      | 3     |
| Larisa     | Savina       | 3     |
| Galina     | Orekhova     | 3     |
| Arina      | SHarapova    | 2     |
| Viktoriya  | Sergeeva     | 2     |
| Vasilij    | Krasilnikov  | 2     |
| Timur      | Rusakov      | 2     |
| Gleb       | Nesterov     | 2     |
| Denis      | Makarov      | 2     |
| Elizaveta  | SHilova      | 2     |
| Vera       | Evseeva      | 1     |
| Margarita  | Kabanova     | 1     |
| Angelina   | Lazareva     | 1     |
| Semyon     | Voronov      | 1     |
| Innokentij | Nekrasov     | 1     |
| Artyom     | Nikitin      | 1     |
| Egor       | Belyakov     | 1     |

To calculate the number of students studying in each class and display this information in a new column, we can use a window function:

```sql
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

| first_name | last_name    | class | student_count_in_class |
| ---------- | ------------ | ----- | ---------------------- |
| Egor       | Belyakov     | 1     | 7                      |
| Artyom     | Nikitin      | 1     | 7                      |
| Innokentij | Nekrasov     | 1     | 7                      |
| Semyon     | Voronov      | 1     | 7                      |
| Angelina   | Lazareva     | 1     | 7                      |
| Margarita  | Kabanova     | 1     | 7                      |
| Vera       | Evseeva      | 1     | 7                      |
| Denis      | Makarov      | 2     | 7                      |
| Arina      | SHarapova    | 2     | 7                      |
| Viktoriya  | Sergeeva     | 2     | 7                      |
| Vasilij    | Krasilnikov  | 2     | 7                      |
| Timur      | Rusakov      | 2     | 7                      |
| Gleb       | Nesterov     | 2     | 7                      |
| Elizaveta  | SHilova      | 2     | 7                      |
| Kirill     | SHubin       | 3     | 8                      |
| YUrij      | Markov       | 3     | 8                      |
| Grigorij   | Kolobov      | 3     | 8                      |
| Semyon     | Trofimov     | 3     | 8                      |
| Valentina  | SHarova      | 3     | 8                      |
| Larisa     | Savina       | 3     | 8                      |
| Galina     | Orekhova     | 3     | 8                      |
| Vasilij    | Ustinov      | 3     | 8                      |
| Timur      | Subbotin     | 4     | 9                      |
| Roman      | SHilov       | 4     | 9                      |
| Danila     | Osipov       | 4     | 9                      |
| Arina      | Silina       | 4     | 9                      |
| Nadezhda   | Zaharova     | 4     | 9                      |
| Larisa     | SHCHerbakova | 4     | 9                      |
| Aleksandra | Belozyorova  | 4     | 9                      |
| Natalya    | Davydova     | 4     | 9                      |
| Mariya     | Fadeeva      | 4     | 9                      |
| Gennadij   | Ovchinnikov  | 5     | 8                      |
| Stanislav  | Lazarev      | 5     | 8                      |
| Viktoriya  | Makarova     | 5     | 8                      |
| Nataliya   | Myasnikova   | 5     | 8                      |
| Valentina  | Sazonova     | 5     | 8                      |
| Valeriya   | Lapina       | 5     | 8                      |
| ZHanna     | Fokina       | 5     | 8                      |
| Anna       | Kulagina     | 5     | 8                      |
| Ilya       | Filippov     | 6     | 11                     |
| Svyatoslav | Muravyov     | 6     | 11                     |
| Denis      | Mel          | 6     | 11                     |
| Matvej     | Bogdanov     | 6     | 11                     |
| YUliya     | Avdeeva      | 6     | 11                     |
| Elizaveta  | Samojlova    | 6     | 11                     |
| Polina     | Sidorova     | 6     | 11                     |
| Lyubov     | Zaharova     | 6     | 11                     |
| Ilya       | Alekseev     | 6     | 11                     |
| Matvej     | YAkushev     | 6     | 11                     |
| Svyatoslav | Tarasov      | 6     | 11                     |
| Nina       | Odincova     | 7     | 10                     |
| Boris      | Simonov      | 7     | 10                     |
| Dmitrij    | Trofimov     | 7     | 10                     |
| YAkov      | Rozhkov      | 7     | 10                     |
| Fyodor     | Drozdov      | 7     | 10                     |
| Gleb       | Strelkov     | 7     | 10                     |
| Angelina   | Lukina       | 7     | 10                     |
| Valeriya   | Novikova     | 7     | 10                     |
| Grigorij   | Kapustin     | 7     | 10                     |
| Vitalij    | Panfilov     | 7     | 10                     |
| Anna       | Osipova      | 8     | 11                     |
| Georgij    | Baranov      | 8     | 11                     |
| YUliya     | Vishnyakova  | 8     | 11                     |
| Valentina  | Bolshakova   | 8     | 11                     |
| Leonid     | Kryukov      | 8     | 11                     |
| Vladislav  | Cvetkov      | 8     | 11                     |
| Lyubov     | Borisova     | 8     | 11                     |
| Anfisa     | Kalashnikova | 8     | 11                     |
| Snezhana   | Morozova     | 8     | 11                     |
| Kristina   | Myasnikova   | 8     | 11                     |
| Kristina   | Smirnova     | 8     | 11                     |
| Vyacheslav | Eliseev      | 9     | 13                     |
| Ivan       | Efremov      | 9     | 13                     |
| Anatolij   | ZHdanov      | 9     | 13                     |
| Georgij    | Noskov       | 9     | 13                     |
| Artyom     | Sergeev      | 9     | 13                     |
| Arina      | Evseeva      | 9     | 13                     |
| Angelina   | Voroncova    | 9     | 13                     |
| Ekaterina  | Ustinova     | 9     | 13                     |
| Raisa      | Lapina       | 9     | 13                     |
| Leonid     | Ignatov      | 9     | 13                     |
| Snezhana   | Seliverstova | 9     | 13                     |
| Semyon     | Biryukov     | 9     | 13                     |
| Nikolaj    | Sokolov      | 9     | 13                     |

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

To wrap up, let's test your understanding:

**What is the key difference between window functions and aggregate functions with grouping in SQL?**

1. Window functions and aggregate functions with grouping perform the same calculations but with different syntax. — Window functions and aggregate functions with grouping have different functionality and cannot be used interchangeably.

2. **Correct answer:** Window functions are calculated independently for each row, returning the result in a separate column. Aggregate functions with grouping, on the other hand, group rows and apply to the formed groups. — Window functions provide calculations for each row, taking into account a set of rows (window) related to the current row, while aggregate functions with grouping provide a single result for each group formed based on the grouping criteria.

3. Window functions use PARTITION BY, while aggregate functions with grouping do not. — Although PARTITION BY is indeed a feature of window functions, the key difference lies in how the functions are applied to the data (by rows versus groups).
