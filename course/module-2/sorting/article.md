---
meta:
    title: 'Sorting, ORDER BY operator'
    description: 'SQL ORDER BY operator, sorting by multiple columns, examples of usage'
---

<ArticleBanner src="https://sql-academy.org/static/guidePage/sorting/banner.jpg" />

# Sorting, ORDER BY operator

When executing a `SELECT` query, the rows are returned by default in an undefined order.
In fact, the order of rows in this case depends on the connection and scanning plan, as well as the order of data placement on the disk,
so it cannot be relied upon. The `ORDER BY` construct is used to order the records.

## General query structure with ORDER BY operator

```sql
SELECT table_fields FROM table_name
WHERE ...
ORDER BY column_1 [ASC | DESC][, column_n [ASC | DESC]]
```

Where `ASC` and `DESC` are sorting directions:

-   `ASC` - sorting in ascending order (by default)
-   `DESC` - sorting in descending order

For example, Let's display the names of airlines in alphabetical order from the `Company` table:

```sql
SELECT name FROM Company ORDER BY name;
```

## Sorting in ascending and descending order for main types

| Data type     | ASC                                                                                                                   | DESC                                                                                                   |
| :------------ | :-------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| String type   | Lexicographic (alphabetical) order from “A” to “Z” <br /> <br /> Records starting with “a” come first, then “b”, etc. | Lexicographic order from “Z” to “A” <br /> <br /> Records starting with “z” come first, then “y”, etc. |
| Numeric type  | From smallest to largest                                                                                              | From largest to smallest                                                                               |
| Date and time | From earlier date/time to later <br /> <br /> For example, first `01.01.2024`, then `01.02.2024`                      | From later date/time to earlier <br /> <br /> For example, first `01.02.2024`, then `01.01.2024`       |
| Boolean type  | `False` comes before `True`                                                                                           | `True` comes before `False`                                                                            |

## Sorting by multiple columns

To sort results by two or more columns, they should be specified separated by commas.

```sql
...ORDER BY column_1 [ASC | DESC], column_2 [ASC | DESC];
```

Data will be sorted by the first column, but in case there are multiple records with identical values in the first column, they will be sorted by the second column.
The number of columns that can be sorted is unlimited.

> The sorting rule applies only to the column that follows it.
>
> `ORDER BY column_1, column_2 DESC`
>
> is not the same as
>
> `ORDER BY column_1 DESC, column_2 DESC`

Let's display flight information sorted by the departure city in ascending order and by the arrival city in descending order, from the `Trip` table:

```sql
SELECT DISTINCT town_from, town_to FROM Trip
ORDER BY town_from, town_to DESC;
```

In this example, the entries are sorted by the `town_from` field first. Then, it performs reverse sorting by the `town_to` field for groups of rows that have the same value in the `town_from` column.

## Demonstration of how sorting works
