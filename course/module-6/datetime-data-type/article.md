---
meta:
    title: 'Date and time in SQL'
    description: 'Functions for working with dates and times in SQL. DATE, TIME, DATETIME, TIMESTAMP data types and their differences.'
---

# Date and Time data types

MySQL has several data types for working with dates and times: `DATE`, `TIME`, `DATETIME` and `TIMESTAMP`.

| Type        | Description                                                                                                                       | Value range                                     | Data type size |
| :---------- | :-------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------- | -------------- |
| `DATE`      | Stores date values as YYYY-MM-DD. <br /> For example, 2022-12-05                                                                  | from 1000-01-01 to 9999-12-31                   | 3 bytes        |
| `TIME`      | Stores time values in the format HH:MM:SS. (or in the format HHH:MM:SS for values with more hours). <br /> For example, 800:50:50 | from -838:59:59 to 838:59:59                    | 3 bytes        |
| `DATETIME`  | Stores a date and time value as YYYY-MM-DD HH:MM:SS. <br /> For example, 2022-12-05 10:37:22                                      | from 1000-01-01 00:00:00 to 9999-12-31 23:59:59 | 8 bytes        |
| `TIMESTAMP` | Stores a date and time value as YYYY-MM-DD HH:MM:SS. <br /> For example, 2022-12-05 10:37:22                                      | from 1970-01-01 00:00:01 to 2038-01-19 03:14:07 | 4 bytes        |

## Difference between TIMESTAMP and DATETIME

The MySQL `DATETIME` and `TIMESTAMP` data types are similar in that they both store dates and times.
But there are a number of significant differences between them that determine which of these types is better to use where.

## DATETIME

Stores values in the range 1000-01-01 00:00:00 to 9999-12-31 23:59:59 and occupies 8 bytes.
This data type does not depend on the time zone, installed in MySQL. It is always displayed exactly like this, in in which it was installed and in which it is stored in the database.
That is, at change the time zone, the time display will not change.

```sql
CREATE TABLE datetime_table (datetime_field DATETIME);
SET @@session.time_zone="+00:00"; -- reset timezone in MYSQL
INSERT INTO datetime_table VALUES("2022-06-16 16:37:23");
SET @@session.time_zone="+03:00"; -- change timezone in MySQL
SELECT * FROM datetime_table;
```

| datetime_field      |
| ------------------- |
| 2022-06-16 16:37:23 |

## TIMESTAMP

Stores how many seconds have passed since 1970-01-01 00:00:00 in the zero time zone and occupies 4 bytes.
When making selections, it is displayed based on the current time zone.
The time zone can be set in the operating room settings. system where MySQL is running, in the global MySQL settings or in a specific session.
In the database, when a `TIMESTAMP` record is created, the value is stored in the zero time zone.

```sql
CREATE TABLE timestamp_table (timestamp_field TIMESTAMP);
SET @@session.time_zone="+00:00"; -- reset timezone in MYSQL
INSERT INTO timestamp_table VALUES("2022-06-16 16:37:23");
SET @@session.time_zone="+03:00"; -- change timezone in MySQL
SELECT * FROM timestamp_table;
```

| timestamp_field     |
| ------------------- |
| 2022-06-16 19:37:23 |

It is also worth remembering that `TIMESTAMP` is essentially limited in the range of possible values from 1970-01-01 00:00:01 to 2038-01-19 03:14:07,
which limits its use. So, this data type is not suitable for storing users dates of birth.

## Ways to set values

The `DATETIME`, `DATE`, and `TIMESTAMP` values can be set in one of the following ways:

- As a string in the format YYYY-MM-DD HH:MM:SS or in the format YY-MM-DD HH:MM:SS for date and time
- As a string in YYYY-MM-DD format or in YY-MM-DD format to indicate dates only

When writing a date, you can use any punctuation mark in as a separator between parts of sections of a date or time. Also it is possible to set the date without a separator character at all, together.

```sql
CREATE TABLE date_table (datetime TIMESTAMP);
INSERT INTO date_table VALUES("2022-06-16 16:37:23");
INSERT INTO date_table VALUES("22.05.31 8+15+04");
INSERT INTO date_table VALUES("2014/02/22 16*37*22");
INSERT INTO date_table VALUES("20220616163723");
INSERT INTO date_table VALUES("2021-02-12");
SELECT * FROM date_table;
```

| datetime            |
| ------------------- |
| 2022-06-16 16:37:23 |
| 2022-05-31 08:15:04 |
| 2014-02-22 16:37:22 |
| 2022-06-16 16:37:23 |
| 2021-02-12 00:00:00 |
