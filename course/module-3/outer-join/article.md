---
meta:
  title: "Outer join"
  description: "Description and syntax of outer join"
---

# Outer Join

An outer join can be of three types: (`LEFT`), (`RIGHT`), and (`FULL`). By default, it is full.

The main difference between an outer join and an inner join is that it must return all rows of one (`LEFT`, `RIGHT`) or both tables (`FULL`).

## LEFT OUTER JOIN

A join that returns all values from the left table joined with corresponding values from the right table
if they satisfy the join condition, or replaces them with `NULL` otherwise.

For example, let's get the schedule of classes from a database, joined with the corresponding time pairs in the schedule.

Data in the `Timepair` table:

Data in the `Schedule` table

```sql-executable-Schedule
SELECT Timepair.id 'timepair.id', start_pair, end_pair,
    Schedule.id 'schedule.id', date, class, number_pair, teacher, subject, classroom
FROM Timepair
    LEFT JOIN Schedule ON Schedule.number_pair = Timepair.id;
```

The selection includes all rows from the left table, supplemented with data about classes.
Note that there are rows at the end of the table with fields filled with `NULL`.
These are the rows for which corresponding classes were not found, but they are present in the left table, so they were also output.

## RIGHT OUTER JOIN

A join that returns all values from the right table joined with corresponding values from the left table if they satisfy the join condition, or replaces them with `NULL` otherwise.

<PostgreSQLOnly>

## FULL OUTER JOIN

A join that performs an inner join of records and supplements them with a left outer join and a right outer join.

The algorithm of the full join:

- A table is formed based on the inner join.
- Values that did not enter the result of the formation from the left table are added to the table
- Values that did not enter the result of the formation from the right table are added to the table

</PostgreSQLOnly>

<MySQLOnly>

## Emulating Full Join in MySQL

Since MySQL does not support `FULL OUTER JOIN`, it can be emulated using `UNION ALL`:

```sql
SELECT *
FROM left_table
LEFT JOIN right_table
   ON right_table.key = left_table.key

UNION ALL

SELECT *
FROM left_table
RIGHT JOIN right_table
ON right_table.key = left_table.key
 WHERE left_table.key IS NULL
```

</MySQLOnly>

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
                <img src="/static/guidePage/multi-table-request-join/left.svg" width="100" />
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
                <img src="/static/guidePage/multi-table-request-join/right.svg" />
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
                <img src="/static/guidePage/multi-table-request-join/left_no_right.svg" />
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
                <img src="/static/guidePage/multi-table-request-join/right_no_left.svg" />
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
                <img src="/static/guidePage/multi-table-request-join/right_and_left.svg" />
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
                <img src="/static/guidePage/multi-table-request-join/all.svg" />
            </td>
            <td>
                Retrieving all data that belongs to both the left and right tables, as well as their inner join:

<PostgreSQLOnly>

```sql
SELECT table_fields
FROM left_table
    FULL OUTER JOIN right_table
    ON right_table.key = left_table.key
```

</PostgreSQLOnly>

<MySQLOnly>

```sql
SELECT table_fields
FROM left_table
LEFT JOIN right_table
    ON right_table.key = left_table.key

UNION ALL

SELECT table_fields
FROM left_table
RIGHT JOIN right_table
    ON right_table.key = left_table.key
WHERE left_table.key IS NULL
```

</MySQLOnly>

            </td>
        </tr>
                <tr>
            <td>
                <img src="/static/guidePage/multi-table-request-join/right_xor_left.svg" />
            </td>
            <td>
                Retrieving data that does not belong to both the left and right tables simultaneously (reverse INNER JOIN):

<PostgreSQLOnly>

```sql
SELECT table_fields
FROM left_table
    FULL OUTER JOIN right_table
    ON right_table.key = left_table.key
WHERE left_table.key IS NULL
    OR right_table.key IS NULL
```

</PostgreSQLOnly>

<MySQLOnly>

```sql
SELECT table_fields
FROM left_table
LEFT JOIN right_table
    ON right_table.key = left_table.key
WHERE right_table.key IS NULL

UNION ALL

SELECT table_fields
FROM left_table
RIGHT JOIN right_table
    ON right_table.key = left_table.key
WHERE left_table.key IS NULL
```

</MySQLOnly>

            </td>
        </tr>
    </tbody>

</Table>
