---
meta:
  title: "Date and time in SQL: MySQL and PostgreSQL"
  description: "Functions for working with dates and times in MySQL and PostgreSQL. DATE, TIME, DATETIME, TIMESTAMP data types and their differences."
---

# Date and Time data types

<MySQLOnly>

MySQL has several data types for working with dates and times: `DATE`, `TIME`, `DATETIME` and `TIMESTAMP`.

| Type        | Description                                                                                                                       | Value range                                     | Data type size |
| :---------- | :-------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------- | -------------- |
| `DATE`      | Stores date values as YYYY-MM-DD. <br /> For example, 2022-12-05                                                                  | from 1000-01-01 to 9999-12-31                   | 3 bytes        |
| `TIME`      | Stores time values in the format HH:MM:SS. (or in the format HHH:MM:SS for values with more hours). <br /> For example, 800:50:50 | from -838:59:59 to 838:59:59                    | 3 bytes        |
| `DATETIME`  | Stores a date and time value as YYYY-MM-DD HH:MM:SS. <br /> For example, 2022-12-05 10:37:22                                      | from 1000-01-01 00:00:00 to 9999-12-31 23:59:59 | 8 bytes        |
| `TIMESTAMP` | Stores a date and time value as YYYY-MM-DD HH:MM:SS. <br /> For example, 2022-12-05 10:37:22                                      | from 1970-01-01 00:00:01 to 2038-01-19 03:14:07 | 4 bytes        |

</MySQLOnly>

<PostgreSQLOnly>

PostgreSQL has several data types for working with dates and times: `DATE`, `TIME`, `TIMESTAMP`, `TIMESTAMPTZ` and `INTERVAL`.

| Type          | Description                                                                           | Value range                              | Data type size |
| :------------ | :------------------------------------------------------------------------------------ | :--------------------------------------- | -------------- |
| `DATE`        | Stores date values as YYYY-MM-DD. <br /> For example, 2022-12-05                      | from 4713 BC to 5874897 AD               | 4 bytes        |
| `TIME`        | Stores time values in the format HH:MM:SS. <br /> For example, 14:30:45               | from 00:00:00 to 24:00:00                | 8 bytes        |
| `TIMESTAMP`   | Stores date and time value without time zone. <br /> For example, 2022-12-05 10:37:22 | from 4713 BC to 294276 AD                | 8 bytes        |
| `TIMESTAMPTZ` | Stores date and time value with time zone. <br /> For example, 2022-12-05 10:37:22+03 | from 4713 BC to 294276 AD                | 8 bytes        |
| `INTERVAL`    | Stores time intervals. <br /> For example, 1 year 2 months 3 days 4 hours             | from -178000000 years to 178000000 years | 16 bytes       |

</PostgreSQLOnly>

<MySQLOnly>

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

It is also worth remembering that `TIMESTAMP` is essentially limited in the range of possible values from 1970-01-01 00:00:01 to 2038-01-19 03:14:07,
which limits its use. So, this data type is not suitable for storing users dates of birth.

</MySQLOnly>

<PostgreSQLOnly>

## Difference between TIMESTAMP and TIMESTAMPTZ

PostgreSQL has two main types for storing date and time: `TIMESTAMP` (without time zone) and `TIMESTAMPTZ` (with time zone).

## TIMESTAMP

The `TIMESTAMP` type (or `TIMESTAMP WITHOUT TIME ZONE`) stores date and time without time zone information.
This means that PostgreSQL does not know what time zone the date was recorded in and does not perform any conversions.

```sql
CREATE TABLE timestamp_table (timestamp_field TIMESTAMP);
SET timezone = 'UTC'; -- set UTC timezone
INSERT INTO timestamp_table VALUES('2022-06-16 16:37:23');
SET timezone = 'Europe/Moscow'; -- change timezone
SELECT * FROM timestamp_table;
```

Result: `2022-06-16 16:37:23` (value did not change)

## TIMESTAMPTZ

The `TIMESTAMPTZ` type (or `TIMESTAMP WITH TIME ZONE`) stores date and time with time zone information.
PostgreSQL automatically converts time to UTC for storage and back to local time zone for output.

```sql
CREATE TABLE timestamptz_table (timestamptz_field TIMESTAMPTZ);
SET timezone = 'UTC'; -- set UTC timezone
INSERT INTO timestamptz_table VALUES('2022-06-16 16:37:23');
SET timezone = 'Europe/Moscow'; -- change timezone to Moscow (+3)
SELECT * FROM timestamptz_table;
```

Result: `2022-06-16 19:37:23+03` (time automatically adjusted)

## INTERVAL

The `INTERVAL` type is used for storing time periods. It can represent intervals in years, months, days, hours, minutes and seconds.

```sql
SELECT '1 year 2 months 3 days 4 hours 5 minutes 6 seconds'::INTERVAL;
SELECT '2 weeks'::INTERVAL;
SELECT '90 minutes'::INTERVAL;
```

</PostgreSQLOnly>

## Ways to set values

<MySQLOnly>

The `DATETIME`, `DATE`, and `TIMESTAMP` values can be set in one of the following ways:

- As a string in the format YYYY-MM-DD HH:MM:SS or in the format YY-MM-DD HH:MM:SS for date and time
- As a string in YYYY-MM-DD format or in YY-MM-DD format to indicate dates only

When writing a date, you can use any punctuation mark in as a separator between parts of sections of a date or time. Also it is possible to set the date without a separator character at all, together.

```sql-executable
CREATE TABLE date_table (datetime TIMESTAMP);
INSERT INTO date_table VALUES("2022-06-16 16:37:23");
INSERT INTO date_table VALUES("22.05.31 8+15+04");
INSERT INTO date_table VALUES("2014/02/22 16*37*22");
INSERT INTO date_table VALUES("20220616163723");
INSERT INTO date_table VALUES("2021-02-12");
SELECT * FROM date_table;
```

</MySQLOnly>

<PostgreSQLOnly>

Date and time values can be set in various formats:

- As a string in ISO 8601 format: 'YYYY-MM-DD HH:MM:SS'
- As a string with time zone: 'YYYY-MM-DD HH:MM:SS+TZ'
- Date only: 'YYYY-MM-DD'
- Time only: 'HH:MM:SS'

```sql-executable
CREATE TABLE date_table (
    date_field DATE,
    time_field TIME,
    timestamp_field TIMESTAMP,
    timestamptz_field TIMESTAMPTZ
);

INSERT INTO date_table VALUES(
    '2022-06-16',
    '16:37:23',
    '2022-06-16 16:37:23',
    '2022-06-16 16:37:23+03'
);

SELECT * FROM date_table;
```

PostgreSQL strictly follows the ISO 8601 standard and prefers to use standard date formats.

</PostgreSQLOnly>
