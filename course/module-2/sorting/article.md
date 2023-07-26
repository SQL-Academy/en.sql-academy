# Sorting, ORDER BY operator

When executing a `SELECT` query, the rows are returned by default in an undefined order.
In fact, the order of rows in this case depends on the connection and scanning plan, as well as the order of data placement on the disk,
so it cannot be relied upon. The `ORDER BY` construct is used to order the records.

## General query structure with ORDER BY operator

```sql
SELECT table_fields FROM table_name
WHERE ...
ORDER BY столбец_1 [ASC | DESC][, столбец_n [ASC | DESC]]
```

Where `ASC` and `DESC` are sorting directions:

- `ASC` - sorting in ascending order (by default)
- `DESC` - sorting in descending order

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

## Usage examples

- Let's display the names of airlines in alphabetical order from the `Company` table:

  > Sorting string data in ascending order implies sorting in lexicographic (alphabetical) order.

  ```sql
  SELECT name FROM Company ORDER BY name;
  ```

  | name       |
  | ---------- |
  | Aeroflot   |
  | air_France |
  | British_AW |
  | Dale_avia  |
  | Don_avia   |

- Let's display flight information sorted by the departure city in ascending order and by the arrival city in descending order, from the `Trip` table:

  ```sql
  SELECT DISTINCT town_from, town_to FROM Trip
  ORDER BY town_from, town_to DESC;
  ```

  | town_from   | town_to     |
  | ----------- | ----------- |
  | London      | Singapore   |
  | London      | Paris       |
  | Moscow      | Rostov      |
  | Paris       | Rostov      |
  | Rostov      | Vladivostok |
  | Rostov      | Paris       |
  | Rostov      | Moscow      |
  | Singapore   | London      |
  | Vladivostok | Rostov      |

  In this example, the entries are sorted by the `town_from` field first. Then, if there are multiple entries with the same value in the `town_from` field, \
  the reverse sorting by the `town_to` field is triggered.
