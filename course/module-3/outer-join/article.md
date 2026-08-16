---
meta:
    title: "Outer join"
    description: "How LEFT, RIGHT and FULL OUTER JOIN work: rows without a match, NULL in the result and why the result grows"
---

# Outer Join

An inner join keeps only the rows that found a match in the other table. An outer join works differently: it always returns every row of one table or of both, filling the missing half with `NULL`.

There are three kinds of outer join: left (`LEFT`), right (`RIGHT`) and full (`FULL`). The kind is mandatory — a bare `OUTER JOIN` is a syntax error. The word `OUTER` itself is optional: `LEFT JOIN` and `LEFT OUTER JOIN` mean exactly the same, and the shorter form is used below.

> Do not confuse this with a bare `JOIN` — that is an inner join, the same as `INNER JOIN`.

## LEFT OUTER JOIN

Returns every row of the left table. If a row finds a match in the right table, the two rows are glued together; if it does not, the right table columns are filled with `NULL`.

For example, let's get the schedule of calls from the database, joined with the corresponding classes in the schedule.

Schedule database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/outer-join).

Data in the `Timepair` table (schedule of calls):

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

Data in the `Schedule` table (schedule of classes):

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
SELECT Timepair.id "timepair.id", start_pair, end_pair,
    Schedule.id "schedule.id", date, class, number_pair, teacher, subject, classroom
FROM Timepair
    LEFT JOIN Schedule ON Schedule.number_pair = Timepair.id;
```

| timepair.id | start_pair | end_pair | schedule.id | date                     | class | number_pair | teacher | subject | classroom |
| ----------- | ---------- | -------- | ----------- | ------------------------ | ----- | ----------- | ------- | ------- | --------- |
| 1           | 08:30:00   | 09:15:00 | 35          | 2019-09-04T00:00:00.000Z | 8     | 1           | 1       | 11      | 4         |
| 1           | 08:30:00   | 09:15:00 | 32          | 2019-09-03T00:00:00.000Z | 8     | 1           | 10      | 10      | 40        |
| 1           | 08:30:00   | 09:15:00 | 21          | 2019-08-30T00:00:00.000Z | 8     | 1           | 7       | 9       | 53        |
| 1           | 08:30:00   | 09:15:00 | 16          | 2019-08-30T00:00:00.000Z | 9     | 1           | 2       | 4       | 34        |
| 1           | 08:30:00   | 09:15:00 | 13          | 2019-09-05T00:00:00.000Z | 9     | 1           | 3       | 13      | 43        |
| 1           | 08:30:00   | 09:15:00 | 10          | 2019-09-04T00:00:00.000Z | 9     | 1           | 9       | 9       | 39        |
| 1           | 08:30:00   | 09:15:00 | 7           | 2019-09-03T00:00:00.000Z | 9     | 1           | 5       | 6       | 36        |
| 1           | 08:30:00   | 09:15:00 | 4           | 2019-09-02T00:00:00.000Z | 9     | 1           | 4       | 3       | 13        |
| 1           | 08:30:00   | 09:15:00 | 1           | 2019-09-01T00:00:00.000Z | 9     | 1           | 11      | 1       | 47        |
| 2           | 09:20:00   | 10:05:00 | 40          | 2019-09-05T00:00:00.000Z | 8     | 2           | 11      | 1       | 43        |
| 2           | 09:20:00   | 10:05:00 | 36          | 2019-09-04T00:00:00.000Z | 8     | 2           | 1       | 12      | 42        |
| 2           | 09:20:00   | 10:05:00 | 33          | 2019-09-03T00:00:00.000Z | 8     | 2           | 7       | 9       | 53        |
| 2           | 09:20:00   | 10:05:00 | 26          | 2019-09-01T00:00:00.000Z | 8     | 2           | 2       | 4       | 34        |
| 2           | 09:20:00   | 10:05:00 | 22          | 2019-08-30T00:00:00.000Z | 8     | 2           | 7       | 9       | 53        |
| 2           | 09:20:00   | 10:05:00 | 17          | 2019-08-30T00:00:00.000Z | 9     | 2           | 8       | 2       | 13        |
| 2           | 09:20:00   | 10:05:00 | 14          | 2019-09-05T00:00:00.000Z | 9     | 2           | 11      | 1       | 47        |
| 2           | 09:20:00   | 10:05:00 | 11          | 2019-09-04T00:00:00.000Z | 9     | 2           | 10      | 10      | 40        |
| 2           | 09:20:00   | 10:05:00 | 8           | 2019-09-03T00:00:00.000Z | 9     | 2           | 13      | 7       | 37        |
| 2           | 09:20:00   | 10:05:00 | 5           | 2019-09-02T00:00:00.000Z | 9     | 2           | 2       | 4       | 34        |
| 2           | 09:20:00   | 10:05:00 | 2           | 2019-09-01T00:00:00.000Z | 9     | 2           | 8       | 2       | 13        |
| 3           | 10:15:00   | 11:00:00 | 37          | 2019-09-04T00:00:00.000Z | 8     | 3           | 3       | 13      | 43        |
| 3           | 10:15:00   | 11:00:00 | 34          | 2019-09-03T00:00:00.000Z | 8     | 3           | 7       | 9       | 53        |
| 3           | 10:15:00   | 11:00:00 | 30          | 2019-09-02T00:00:00.000Z | 8     | 3           | 6       | 8       | 38        |
| 3           | 10:15:00   | 11:00:00 | 27          | 2019-09-01T00:00:00.000Z | 8     | 3           | 6       | 5       | 35        |
| 3           | 10:15:00   | 11:00:00 | 23          | 2019-08-30T00:00:00.000Z | 8     | 3           | 8       | 2       | 38        |
| 3           | 10:15:00   | 11:00:00 | 18          | 2019-08-30T00:00:00.000Z | 9     | 3           | 6       | 5       | 35        |
| 3           | 10:15:00   | 11:00:00 | 15          | 2019-09-05T00:00:00.000Z | 9     | 3           | 5       | 6       | 36        |
| 3           | 10:15:00   | 11:00:00 | 12          | 2019-09-04T00:00:00.000Z | 9     | 3           | 3       | 11      | 41        |
| 3           | 10:15:00   | 11:00:00 | 9           | 2019-09-03T00:00:00.000Z | 9     | 3           | 6       | 8       | 38        |
| 3           | 10:15:00   | 11:00:00 | 6           | 2019-09-02T00:00:00.000Z | 9     | 3           | 6       | 5       | 35        |
| 3           | 10:15:00   | 11:00:00 | 3           | 2019-09-01T00:00:00.000Z | 9     | 3           | 4       | 3       | 13        |
| 4           | 11:05:00   | 11:50:00 | 38          | 2019-09-04T00:00:00.000Z | 8     | 4           | 8       | 2       | 42        |
| 4           | 11:05:00   | 11:50:00 | 31          | 2019-09-02T00:00:00.000Z | 8     | 4           | 7       | 9       | 53        |
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

All eight calls made it into the result, exactly as a left join promises. But the result has 43 rows, not 8.

A join does not supplement the left table — it goes through every matching pair of rows. The same pair number appears in the schedule many times, on different days and for different classes, and every match produces its own row. When the key is not unique in the right table, the result has more rows than the left table.

At the end of the result there are rows where every class column is `NULL`. These are the calls with no class at all: there is no match, but a row of the left table is guaranteed to appear in the result.

### Rows without a match

Those `NULL` values are the basis of the most common practical trick — finding records that have no match. It is enough to keep only the rows where the key of the right table is empty:

```sql
SELECT Timepair.id, start_pair, end_pair
FROM Timepair
    LEFT JOIN Schedule ON Schedule.number_pair = Timepair.id
WHERE Schedule.number_pair IS NULL;
```

| id  | start_pair | end_pair |
| --- | ---------- | -------- |
| 6   | 13:40:00   | 14:25:00 |
| 7   | 14:35:00   | 15:20:00 |
| 8   | 15:25:00   | 16:10:00 |

Three calls are left — the ones with no class scheduled.

A join from which only the rows without a match are kept is called an **anti join** (`ANTI JOIN`). It has no operator of its own: both in MySQL and in PostgreSQL it is written exactly like this — a join plus an `IS NULL` condition.

## RIGHT OUTER JOIN

The mirror image of the left join: every row of the right table is guaranteed to appear in the result, and the missing columns of the left table are filled with `NULL`.

```sql
SELECT Timepair.id "timepair.id", start_pair, end_pair,
    Schedule.id "schedule.id", date, class, number_pair, teacher, subject, classroom
FROM Timepair
    RIGHT JOIN Schedule ON Schedule.number_pair = Timepair.id;
```

The result has 40 rows — exactly as many as there are records in `Schedule` — and not a single row with `NULL`. In other words, it matches the inner join completely.

That happened because every class refers to an existing call: the right table simply has no unmatched rows. The kind of join sets the rule, but what ends up in the result is decided by the data.

## FULL OUTER JOIN

Returns every row of both tables. Rows that found a match are glued together, while unmatched rows of the left and right tables appear in the result with `NULL` instead of the missing half.

The result of a full join is made of three parts:

- the rows of the inner join (`INNER JOIN`);
- the rows of the left table that found no match;
- the rows of the right table that found no match.

**PostgreSQL**

```sql
SELECT Timepair.id "timepair.id", start_pair, end_pair,
    Schedule.id "schedule.id", date, class, number_pair, teacher, subject, classroom
FROM Timepair
    FULL OUTER JOIN Schedule ON Schedule.number_pair = Timepair.id;
```

On the data of this database the result matches the left join — the same 43 rows: here the unmatched rows exist only on the left.

**MySQL**

MySQL does not support `FULL OUTER JOIN`, but the same result can be assembled by hand: take the left join and add the rows of the right table that found no match.

```sql
SELECT table_fields
FROM left_table
    LEFT JOIN right_table ON right_table.key = left_table.key

UNION ALL

SELECT table_fields
FROM left_table
    RIGHT JOIN right_table ON right_table.key = left_table.key
WHERE left_table.key IS NULL;
```

The condition in the second part is required: without it the matched rows would appear in the result twice.

## All types of table joins

| Join type         | Result                              | Rows in the example |
| ----------------- | ----------------------------------- | ------------------: |
| `LEFT JOIN`       | All rows of the left table          |                   4 |
| `INNER JOIN`      | Only matching rows                  |                   3 |
| `RIGHT JOIN`      | All rows of the right table         |                   4 |
| `FULL JOIN`       | All rows of both tables             |                   5 |
| `LEFT ANTI JOIN`  | Left table without a match          |                   1 |
| `RIGHT ANTI JOIN` | Right table without a match         |                   1 |
| `FULL ANTI JOIN`  | Everything except the matching rows |                   2 |

Let's check yourself. The left table has 8 rows. How many rows will a `LEFT JOIN` with the right table return?

1. **Correct answer:** Eight or more — it depends on how many matches were found — Every row of the left table is guaranteed to appear in the result, but a row with several matches in the right table produces several rows.

2. Exactly eight: a left join returns all rows of the left table — All eight rows do appear in the result, but a row with several matches is multiplied — the result grows beyond eight rows.

3. No more than eight: the extra rows of the right table are dropped — Only the unmatched rows of the right table are dropped. Matches are not dropped: each one produces its own row of the result.
