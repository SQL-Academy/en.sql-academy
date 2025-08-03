---
meta:
  title: "Date and time in SQL: functions YEAR, MONTH, TIMESTAMPDIFF, STR_TO_DATE"
  description: "Working with date and time in SQL: functions YEAR, MONTH, DAY, TIMESTAMPDIFF, STR_TO_DATE, EXTRACT. Data types DATE, TIME, DATETIME, TIMESTAMP in MySQL and PostgreSQL."
---

<DBMSSwitcher />

# Date and time in SQL

Among all the data types in SQL, date and time data is the most complex 🤯.
The complexity arises due to several reasons, and here are some of them:

- many ways to set the date and time
- availability of time zones
- non-obviousness of calculations of some values ​​based on date and time.
  For example, the complexity of calculating age.

## Generation of date and time data

Date and time data can be retrieved in one of the following ways:

- copy data from existing column with date and time type
- set date and time via string representation
- get date and time by calling built-in functions that return a date and time data type

### String representation of date and time

The following formats are used to set the date and time:

<MySQLOnly>

| Type        | Default Format                                                                                                                                                     |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `DATE`      | `YYYY-MM-DD`                                                                                                                                                       |
| `DATETIME`  | `YYYY-MM-DD hh:mm:ss`                                                                                                                                              |
| `TIMESTAMP` | `YYYY-MM-DD hh:mm:ss`                                                                                                                                              |
| `TIME`      | `hhh:mm:sss`                                                                                                                                                       |
| `YEAR`      | `YYYY` - full format <br /> `YY` or `Y` - shorthand format that returns a year between 2000-2069 for values ​​0-69 and a year between 1970-1999 for values ​​70-99 |

</MySQLOnly>

<PostgreSQLOnly>

| Type        | Default Format        |
| :---------- | :-------------------- |
| `DATE`      | `YYYY-MM-DD`          |
| `TIMESTAMP` | `YYYY-MM-DD hh:mm:ss` |
| `TIME`      | `hh:mm:ss`            |

</PostgreSQLOnly>

Moreover, when specifying a date, it is allowed to use any punctuation mark as a separator between parts of the date or time sections.
It is also possible to set the date without a separator character at all, together.

Examples of valid setting of values ​​for date and time via string representation:

<MySQLOnly>

```sql-executable
SELECT  CAST("2022-06-16 16:37:23" AS DATETIME) AS datetime_1,
        CAST("2014/02/22 16*37*22" AS DATETIME) AS datetime_2,
        CAST("20220616163723" AS DATETIME) AS datetime_3,
        CAST("2021-02-12" AS DATE) AS date_1,
        CAST("160:23:13" AS TIME) AS time_1,
        CAST("89" AS YEAR) AS year
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
SELECT  CAST('2022-06-16 16:37:23' AS TIMESTAMP) AS timestamp_1,
        CAST('2014/02/22 16:37:22' AS TIMESTAMP) AS timestamp_2,
        CAST('20220616163723' AS TIMESTAMP) AS timestamp_3,
        CAST('2021-02-12' AS DATE) AS date_1,
        CAST('16:23:13' AS TIME) AS time_1
```

</PostgreSQLOnly>

In the query above, the `CAST` function was used to force the string to be converted to a date and time.
It is needed if the server does not expect the date and time and, accordingly, does not automatically convert the string
to the correct type. We'll learn more about type conversion in <a href="/guide/type-conversion-functions" target="_blank">"Type conversion functions, CAST"</a>.

### Date generation functions

If you need to get the date and time from a string that does not match any format that accepts
the `CAST` function, you can use special functions for parsing dates.

<MySQLOnly>

MySQL has a built-in `STR_TO_DATE` function, which takes an arbitrary string containing a date and a format describing it.

```sql-executable
SELECT STR_TO_DATE('November 13, 1998', '%M %d, %Y') AS date;
```

For a more detailed description of the `STR_TO_DATE` function and its arguments, see <a href="/handbook/mysql/str_to_date" target="_blank">in the reference</a>.

</MySQLOnly>

<PostgreSQLOnly>

PostgreSQL has a built-in `TO_DATE` function, which takes an arbitrary string containing a date and a format describing it.

```sql-executable
SELECT TO_DATE('November 13, 1998', 'Month DD, YYYY') AS date;
```

For a more detailed description of the `TO_DATE` function and its arguments, see <a href="/handbook/postgresql/to_date" target="_blank">in the reference</a>.

</PostgreSQLOnly>

To generate the current date or time, there is no need to create a string for its subsequent conversion to a date,
because there are built-in functions for getting given values.

<MySQLOnly>

In MySQL these are the `CURDATE`, `CURTIME` and `NOW` functions.

```sql-executable
SELECT CURDATE(), CURTIME(), NOW();
```

</MySQLOnly>

<PostgreSQLOnly>

In PostgreSQL these are the `CURRENT_DATE`, `CURRENT_TIME` and `NOW` functions.

```sql-executable
SELECT CURRENT_DATE, CURRENT_TIME, NOW();
```

</PostgreSQLOnly>

## Date and Time Extraction Functions

Sometimes it is necessary to obtain information not about the full date, but about its specific part, for example,
about its month or year.

<MySQLOnly>

To do this, SQL has the following functions:

| Function                                                      | Description                                                                  |
| :------------------------------------------------------------ | :--------------------------------------------------------------------------- |
| <a href="/handbook/mysql/year" target="_blank">`YEAR`</a>     | Returns the year for the specified date                                      |
| <a href="/handbook/mysql/month" target="_blank">`MONTH`</a>   | Returns the numeric value of the month of the year (from 1 to 12) for a date |
| <a href="/handbook/mysql/day" target="_blank">`DAY`</a>       | Returns the ordinal number of the day in the month (from 1 to 31)            |
| <a href="/handbook/mysql/hour" target="_blank">`HOUR`</a>     | Returns the hour value (between 0 and 23) for the time                       |
| <a href="/handbook/mysql/minute" target="_blank">`MINUTE`</a> | Returns the minutes value (from 0 to 59) for the time                        |

</MySQLOnly>

<PostgreSQLOnly>

To do this, PostgreSQL uses the `EXTRACT` function:

| Function                                                                               | Description                                                                  |
| :------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------- |
| <a href="/handbook/postgresql/extract" target="_blank">`EXTRACT(YEAR FROM date)`</a>   | Returns the year for the specified date                                      |
| <a href="/handbook/postgresql/extract" target="_blank">`EXTRACT(MONTH FROM date)`</a>  | Returns the numeric value of the month of the year (from 1 to 12) for a date |
| <a href="/handbook/postgresql/extract" target="_blank">`EXTRACT(DAY FROM date)`</a>    | Returns the ordinal number of the day in the month (from 1 to 31)            |
| <a href="/handbook/postgresql/extract" target="_blank">`EXTRACT(HOUR FROM time)`</a>   | Returns the hour value (between 0 and 23) for the time                       |
| <a href="/handbook/postgresql/extract" target="_blank">`EXTRACT(MINUTE FROM time)`</a> | Returns the minutes value (from 0 to 59) for the time                        |

</PostgreSQLOnly>

## The difference between DATETIME and TIMESTAMP

<MySQLOnly>

MySQL has very similar data types: `DATETIME` and `TIMESTAMP`. Both are aimed at storing the date and time.
But they have a number of differences that determine which of these data types is best to use when.

| Criteria  | `DATETIME`                                                 | `TIMESTAMP`                                                                                                                        |
| :-------- | :--------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Range     | from `1000-01-01 00:00:00` <br /> to `9999-12-31 23:59:59` | from `1970-01-01 00:00:00` <br /> to `2038-01-19 03:14:07`                                                                         |
| Time zone | Ignored <br /> Displayed as the date was set               | Taken into account <br /> When making selections, it is displayed taking into account the current time zone of the database server |

</MySQLOnly>

<PostgreSQLOnly>

PostgreSQL has the main types for storing date and time: `TIMESTAMP` (without time zone) and `TIMESTAMPTZ` (with time zone).

| Criteria  | `TIMESTAMP`                                  | `TIMESTAMPTZ`                                                                                                                      |
| :-------- | :------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Range     | from `4713 BC` to `294276 AD`                | from `4713 BC` to `294276 AD`                                                                                                      |
| Time zone | Ignored <br /> Displayed as the date was set | Taken into account <br /> When making selections, it is displayed taking into account the current time zone of the database server |

</PostgreSQLOnly>

## Time zones

Since people all over the world want noon to approximate the maximum rise of the Sun, there has never been a problem
to use universal time and the world was divided into 24 time zones.

UTC (Coordinated Universal Time) is currently used as the time reference point. All other time zones can be described
by the number of hours offset from UTC. For example, the Moscow time zone can be described as UTC+3.

The time zone is one of the database server settings and can be set:

- globally
- for the current user
- for the current user session

<MySQLOnly>

```sql
SET GLOBAL time_zone = '+03:00';    // globally
SET time_zone = '+03:00';           // for the current user
SET @@session.time_zone = '+03:00'; // for the current user session
```

Accordingly, when changing the time zone, all values ​​of the `TIMESTAMP` type will be displayed taking into account the current active time zone.

</MySQLOnly>

<PostgreSQLOnly>

```sql
ALTER DATABASE mydb SET timezone = 'Europe/Moscow';  // globally for database
ALTER USER myuser SET timezone = 'Europe/Moscow';    // for specific user
SET TIME ZONE 'Europe/Moscow';                       // for current session
SET TIME ZONE '+03:00';                              // for current session
```

Accordingly, when changing the time zone, all values ​​of the `TIMESTAMPTZ` type will be displayed taking into account the current active time zone.

</PostgreSQLOnly>

## Examples of tasks for date and time

I would like to pay special attention to the most popular tasks related to the temporary data type,
where mistakes are often made.

### Age determination

When setting the task to find the age of a person by the date of his birth, there is often a temptation 😈 to calculate
the difference between the current year and the person's year of birth:

<MySQLOnly>

```sql-executable
SELECT YEAR(NOW()) - YEAR('2003-07-03 14:10:26');
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
SELECT EXTRACT(YEAR FROM NOW()) - EXTRACT(YEAR FROM TIMESTAMP '2003-07-03 14:10:26');
```

</PostgreSQLOnly>

The problem with this approach is that it does not take into account whether the person had a birthday this year or not yet.
That is, if at the time of the request it was already July 3rd (07-03), then the person celebrated his birthday and he is already 20 years old,
otherwise he is still 19 years old.
The difference in functions will be useless here - in both cases it will give 20 years.

If determining the age in terms of the difference in years is a non-working option, then you may want to find the age in terms of
the difference of days between two dates, then divide this difference by the number of days in a year and round down:

<MySQLOnly>

```sql-executable
SELECT FLOOR(DATEDIFF(NOW(), '2003-07-03 14:10:26') / 365);
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
SELECT FLOOR(EXTRACT(DAY FROM NOW() - TIMESTAMP '2003-07-03 14:10:26') / 365);
```

</PostgreSQLOnly>

And this solution will be much more accurate than the previous one. But it will not be absolutely accurate due to the presence of leap years, when there are 366 days in a year.
Although the error in calculating the age for 1 person due to the presence of a leap year is quite small, in the calculations for determining, say,
average age among a certain list of people, the error can accumulate and distort the real values.

And how then is it correct to determine the age?

<MySQLOnly>

There is a built-in function for this - <a href="/handbook/mysql/timestampdiff" target="_blank">`TIMESTAMPDIFF`</a>,
which takes as its first argument the unit in which to return the difference between two time values.

```sql-executable
SELECT TIMESTAMPDIFF(YEAR, '2003-07-03 14:10:26', NOW());
```

</MySQLOnly>

<PostgreSQLOnly>

For this purpose, the <a href="/handbook/postgresql/extract" target="_blank">`EXTRACT`</a> function is used together with the <a href="/handbook/postgresql/age" target="_blank">`AGE`</a> function,
which calculates the exact interval between two dates.

```sql-executable
SELECT EXTRACT(YEAR FROM AGE(NOW(), TIMESTAMP '2003-07-03 14:10:26'));
```

</PostgreSQLOnly>
