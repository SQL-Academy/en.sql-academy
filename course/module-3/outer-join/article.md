---
meta:
    title: 'Outer join'
    description: 'Description and syntax of outer join'
---

# Outer Join

An outer join can be of three types: (`LEFT`), (`RIGHT`), and (`FULL`). By default, it is full.

The main difference between an outer join and an inner join is that it must return all rows of one (`LEFT`, `RIGHT`) or both tables (`FULL`).

## LEFT OUTER JOIN

A join that returns all values from the left table joined with corresponding values from the right table
if they satisfy the join condition, or replaces them with `NULL` otherwise.

For example, let's get the schedule of classes from a database, joined with the corresponding time pairs in the schedule.

Data in the `Timepair` table:

| id  | start_pair | end_pair |
| --- | ---------- | -------- |
| 1   | 08:30:00   | 09:15:00 |
| 2   | 09:20:00   | 10:05:00 |
| 3   | 10:15:00   | 11:00:00 |
| 4   | 11:05:00   | 11:50:00 |
| 5   | 12:50:00   | 13:35:00 |
| 6   | 13:40:00   | 14:25:00 |
| 7   | 14:35:00   | 15:20:00 |
| 8   | 15:25:00   | 16:10:00 |

Data in the `Schedule` table

| id  | date                     | class | number_pair | teacher | subject | classroom |
| --- | ------------------------ | ----- | ----------- | ------- | ------- | --------- |
| 1   | 2019-09-01T00:00:00.000Z | 9     | 1           | 11      | 1       | 47        |
| 2   | 2019-09-01T00:00:00.000Z | 9     | 2           | 8       | 2       | 13        |
| 3   | 2019-09-01T00:00:00.000Z | 9     | 3           | 4       | 3       | 13        |
| 4   | 2019-09-02T00:00:00.000Z | 9     | 1           | 4       | 3       | 13        |
| 5   | 2019-09-02T00:00:00.000Z | 9     | 2           | 2       | 4       | 34        |
| 6   | 2019-09-02T00:00:00.000Z | 9     | 3           | 6       | 5       | 35        |
| 7   | 2019-09-03T00:00:00.000Z | 9     | 1           | 5       | 6       | 36        |
| 8   | 2019-09-03T00:00:00.000Z | 9     | 2           | 13      | 7       | 37        |
| 9   | 2019-09-03T00:00:00.000Z | 9     | 3           | 6       | 8       | 38        |
| 10  | 2019-09-04T00:00:00.000Z | 9     | 1           | 9       | 9       | 39        |
| 11  | 2019-09-04T00:00:00.000Z | 9     | 2           | 10      | 10      | 40        |
| 12  | 2019-09-04T00:00:00.000Z | 9     | 3           | 3       | 11      | 41        |
| 13  | 2019-09-05T00:00:00.000Z | 9     | 1           | 3       | 13      | 43        |
| 14  | 2019-09-05T00:00:00.000Z | 9     | 2           | 11      | 1       | 47        |
| 15  | 2019-09-05T00:00:00.000Z | 9     | 3           | 5       | 6       | 36        |
| 16  | 2019-08-30T00:00:00.000Z | 9     | 1           | 2       | 4       | 34        |
| 17  | 2019-08-30T00:00:00.000Z | 9     | 2           | 8       | 2       | 13        |
| 18  | 2019-08-30T00:00:00.000Z | 9     | 3           | 6       | 5       | 35        |
| 19  | 2019-08-30T00:00:00.000Z | 9     | 4           | 10      | 1       | 47        |
| 20  | 2019-09-03T00:00:00.000Z | 9     | 4           | 10      | 10      | 40        |
| 21  | 2019-08-30T00:00:00.000Z | 8     | 1           | 7       | 9       | 53        |
| 22  | 2019-08-30T00:00:00.000Z | 8     | 2           | 7       | 9       | 53        |
| 23  | 2019-08-30T00:00:00.000Z | 8     | 3           | 8       | 2       | 38        |
| 24  | 2019-08-30T00:00:00.000Z | 8     | 4           | 11      | 1       | 43        |
| 25  | 2019-08-30T00:00:00.000Z | 8     | 5           | 8       | 3       | 39        |
| 26  | 2019-09-01T00:00:00.000Z | 8     | 2           | 2       | 4       | 34        |
| 27  | 2019-09-01T00:00:00.000Z | 8     | 3           | 6       | 5       | 35        |
| 28  | 2019-09-01T00:00:00.000Z | 8     | 4           | 12      | 6       | 36        |
| 29  | 2019-09-01T00:00:00.000Z | 8     | 5           | 13      | 7       | 37        |
| 30  | 2019-09-02T00:00:00.000Z | 8     | 3           | 6       | 8       | 38        |
| 31  | 2019-09-02T00:00:00.000Z | 8     | 4           | 7       | 9       | 53        |
| 32  | 2019-09-03T00:00:00.000Z | 8     | 1           | 10      | 10      | 40        |
| 33  | 2019-09-03T00:00:00.000Z | 8     | 2           | 7       | 9       | 53        |
| 34  | 2019-09-03T00:00:00.000Z | 8     | 3           | 7       | 9       | 53        |
| 35  | 2019-09-04T00:00:00.000Z | 8     | 1           | 1       | 11      | 4         |
| 36  | 2019-09-04T00:00:00.000Z | 8     | 2           | 1       | 12      | 42        |
| 37  | 2019-09-04T00:00:00.000Z | 8     | 3           | 3       | 13      | 43        |
| 38  | 2019-09-04T00:00:00.000Z | 8     | 4           | 8       | 2       | 42        |
| 39  | 2019-09-04T00:00:00.000Z | 8     | 5           | 11      | 1       | 43        |
| 40  | 2019-09-05T00:00:00.000Z | 8     | 2           | 11      | 1       | 43        |

```sql
SELECT Timepair.id 'timepair.id', start_pair, end_pair,
    Schedule.id 'schedule.id', date, class, number_pair, teacher, subject, classroom
FROM Timepair
    LEFT JOIN Schedule ON Schedule.number_pair = Timepair.id;
```

| timepair.id | start_pair | end_pair | schedule.id | date                     | class | number_pair | teacher | subject | classroom |
| ----------- | ---------- | -------- | ----------- | ------------------------ | ----- | ----------- | ------- | ------- | --------- |
| 1           | 08:30:00   | 09:15:00 | 35          | 2019-09-04T00:00:00      | 8     | 1           | 1       | 11      | 4         |
| 1           | 08:30:00   | 09:15:00 | 32          | 2019-09-03T00:00:00      | 8     | 1           | 10      | 10      | 40        |
| 1           | 08:30:00   | 09:15:00 | 21          | 2019-08-30T00:00:00      | 8     | 1           | 7       | 9       | 53        |
| 1           | 08:30:00   | 09:15:00 | 16          | 2019-08-30T00:00:00      | 9     | 1           | 2       | 4       | 34        |
| 1           | 08:30:00   | 09:15:00 | 13          | 2019-09-05T00:00:00      | 9     | 1           | 3       | 13      | 43        |
| 1           | 08:30:00   | 09:15:00 | 10          | 2019-09-04T00:00:00      | 9     | 1           | 9       | 9       | 39        |
| 1           | 08:30:00   | 09:15:00 | 7           | 2019-09-03T00:00:00      | 9     | 1           | 5       | 6       | 36        |
| 1           | 08:30:00   | 09:15:00 | 4           | 2019-09-02T00:00:00      | 9     | 1           | 4       | 3       | 13        |
| 1           | 08:30:00   | 09:15:00 | 1           | 2019-09-01T00:00:00      | 9     | 1           | 11      | 1       | 47        |
| 2           | 09:20:00   | 10:05:00 | 40          | 2019-09-05T00:00:00      | 8     | 2           | 11      | 1       | 43        |
| 2           | 09:20:00   | 10:05:00 | 36          | 2019-09-04T00:00:00      | 8     | 2           | 1       | 12      | 42        |
| 2           | 09:20:00   | 10:05:00 | 33          | 2019-09-03T00:00:00      | 8     | 2           | 7       | 9       | 53        |
| 2           | 09:20:00   | 10:05:00 | 26          | 2019-09-01T00:00:00      | 8     | 2           | 2       | 4       | 34        |
| 2           | 09:20:00   | 10:05:00 | 22          | 2019-08-30T00:00:00      | 8     | 2           | 7       | 9       | 53        |
| 2           | 09:20:00   | 10:05:00 | 17          | 2019-08-30T00:00:00      | 9     | 2           | 8       | 2       | 13        |
| 2           | 09:20:00   | 10:05:00 | 14          | 2019-09-05T00:00:00      | 9     | 2           | 11      | 1       | 47        |
| 2           | 09:20:00   | 10:05:00 | 11          | 2019-09-04T00:00:00      | 9     | 2           | 10      | 10      | 40        |
| 2           | 09:20:00   | 10:05:00 | 8           | 2019-09-03T00:00:00      | 9     | 2           | 13      | 7       | 37        |
| 2           | 09:20:00   | 10:05:00 | 5           | 2019-09-02T00:00:00      | 9     | 2           | 2       | 4       | 34        |
| 2           | 09:20:00   | 10:05:00 | 2           | 2019-09-01T00:00:00      | 9     | 2           | 8       | 2       | 13        |
| 3           | 10:15:00   | 11:00:00 | 37          | 2019-09-04T00:00:00      | 8     | 3           | 3       | 13      | 43        |
| 3           | 10:15:00   | 11:00:00 | 34          | 2019-09-03T00:00:00      | 8     | 3           | 7       | 9       | 53        |
| 3           | 10:15:00   | 11:00:00 | 30          | 2019-09-02T00:00:00      | 8     | 3           | 6       | 8       | 38        |
| 3           | 10:15:00   | 11:00:00 | 27          | 2019-09-01T00:00:00      | 8     | 3           | 6       | 5       | 35        |
| 3           | 10:15:00   | 11:00:00 | 23          | 2019-08-30T00:00:00      | 8     | 3           | 8       | 2       | 38        |
| 3           | 10:15:00   | 11:00:00 | 18          | 2019-08-30T00:00:00      | 9     | 3           | 6       | 5       | 35        |
| 3           | 10:15:00   | 11:00:00 | 15          | 2019-09-05T00:00:00      | 9     | 3           | 5       | 6       | 36        |
| 3           | 10:15:00   | 11:00:00 | 12          | 2019-09-04T00:00:00      | 9     | 3           | 3       | 11      | 41        |
| 3           | 10:15:00   | 11:00:00 | 9           | 2019-09-03T00:00:00      | 9     | 3           | 6       | 8       | 38        |
| 3           | 10:15:00   | 11:00:00 | 6           | 2019-09-02T00:00:00      | 9     | 3           | 6       | 5       | 35        |
| 3           | 10:15:00   | 11:00:00 | 3           | 2019-09-01T00:00:00      | 9     | 3           | 4       | 3       | 13        |
| 4           | 11:05:00   | 11:50:00 | 38          | 2019-09-04T00:00:00      | 8     | 4           | 8       | 2       | 42        |
| 4           | 11:05:00   | 11:50:00 | 31          | 2019-09-02T00:00:00      | 8     | 4           | 7       | 9       | 53        |
| 4           | 11:05:00   | 11:50:00 | 28          | 2019-09-01T00:00:00.000Z | 8     | 4           | 12      | 6       | 36        |
| 4           | 11:05:00   | 11:50:00 | 24          | 2019-08-30T00:00:00.000Z | 8     | 4           | 11      | 1       | 43        |
| 4           | 11:05:00   | 11:50:00 | 20          | 2019-09-03T00:00:00.000Z | 9     | 4           | 10      | 10      | 40        |
| 4           | 11:05:00   | 11:50:00 | 19          | 2019-08-30T00:00:00.000Z | 9     | 4           | 10      | 1       | 47        |
| 5           | 12:50:00   | 13:35:00 | 39          | 2019-09-04T00:00:00.000Z | 8     | 5           | 11      | 1       | 43        |
| 5           | 12:50:00   | 13:35:00 | 29          | 2019-09-01T00:00:00.000Z | 8     | 5           | 13      | 7       | 37        |
| 5           | 12:50:00   | 13:35:00 | 25          | 2019-08-30T00:00:00.000Z | 8     | 5           | 8       | 3       | 39        |
| 6           | 13:40:00   | 14:25:00 | null        | null                     | null  | null        | null    | null    | null      |
| 7           | 14:35:00   | 15:20:00 | null        | null                     | null  | null        | null    | null    | null      |
| 8           | 15:25:00   | 16:10:00 | null        | null                     | null  | null        | null    | null    | null      |

The selection includes all rows from the left table, supplemented with data about classes.
Note that there are rows at the end of the table with fields filled with `NULL`.
These are the rows for which corresponding classes were not found, but they are present in the left table, so they were also output.

## RIGHT OUTER JOIN

A join that returns all values from the right table joined with corresponding values from the left table if they satisfy the join condition, or replaces them with `NULL` otherwise.

## FULL OUTER JOIN

A join that performs an inner join of records and supplements them with a left outer join and a right outer join.

The algorithm of the full join:

- A table is formed based on the inner join.
- Values that did not enter the result of the formation from the left table are added to the table
- Values that did not enter the result of the formation from the right table are added to the table

> Full join is not implemented in all DBMS. For example, it is not present in MySQL, but it can be easily emulated:
>
> ```sql
> SELECT *
> FROM left_table
> LEFT JOIN right_table
>    ON right_table.key = left_table.key
>
> UNION ALL
>
> SELECT *
> FROM left_table
> RIGHT JOIN right_table
> ON right_table.key = left_table.key
>  WHERE left_table.key IS NULL
> ```

## Basic queries for different types of table joins:

<Table>
    <thead>
        <tr>
            <th>Schema</th>
            <th>JOIN query</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/left.svg" width="100" />
            </td>
            <td>
                   Retrieving all data from the left table joined with corresponding data from the right table:
                ```sql 
                SELECT table_fields 
                FROM left_table LEFT JOIN right_table 
                    ON right_table.key = left_table.key 
                ```
            </td>
        </tr>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/right.svg" />
            </td>
            <td>
                 Retrieving all data from the right table joined with corresponding data from the left table:
                ```sql 
                SELECT table_fields
                FROM left_table RIGHT JOIN right_table
                    ON right_table.key = left_table.key
                ```
            </td>
        </tr>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/left_no_right.svg" />
            </td>
            <td>
                Retrieving data that only belongs to the left table:
                ```sql 
                SELECT table_fields
                FROM left_table LEFT JOIN right_table
                    ON right_table.key = left_table.key
                WHERE right_table.key IS NULL
                ```
            </td>
        </tr>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/right_no_left.svg" />
            </td>
            <td>
                Retrieving data that only belongs to the right table:
                ```sql 
                SELECT table_fields
                FROM left_table RIGHT JOIN right_table
                    ON right_table.key = left_table.key
                WHERE left_table.key IS NULL
                ```
            </td>
        </tr>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/right_and_left.svg" />
            </td>
            <td>
                Retrieving data that belongs to both the left and right tables:
                ```sql 
                SELECT table_fields
                FROM left_table INNER JOIN right_table
                    ON right_table.key = left_table.key
                ```
            </td>
        </tr>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/all.svg" />
            </td>
            <td>
                Retrieving all data that belongs to both the left and right tables, as well as their inner join:
                ```sql 
                SELECT table_fields 
                FROM left_table
                    FULL OUTER JOIN right_table
                    ON right_table.key = left_table.key
                ```
            </td>
        </tr>
        <tr>
            <td>
                <img src="https://sql-academy.org/static/guidePage/multi-table-request-join/right_xor_left.svg" />
            </td>
            <td>
                Retrieving data that does not belong to both the left and right tables simultaneously (reverse INNER JOIN):
                ```sql 
                SELECT table_fields 
                FROM left_table
                    FULL OUTER JOIN right_table
                    ON right_table.key = left_table.key
                WHERE left_table.key IS NULL
                    OR right_table.key IS NULL
                ```
            </td>
        </tr>
    </tbody>

</Table>
