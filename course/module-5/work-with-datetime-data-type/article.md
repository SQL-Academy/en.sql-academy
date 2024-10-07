---
meta:
    title: 'Date and time in SQL'
    description: 'Generating time and date in SQL, string representation of datetime values. Calculating age in SQL.'
---

# Date and time in SQL

Among all the data types in SQL, date and time data is the most complex 🤯.
The complexity arises due to several reasons, and here are some of them:

-   many ways to set the date and time
-   availability of time zones
-   non-obviousness of calculations of some values ​​based on date and time.
    For example, the complexity of calculating age.

## Generation of date and time data

Date and time data can be retrieved in one of the following ways:

-   copy data from existing column with date and time type
-   set date and time via string representation
-   get date and time by calling built-in functions that return a date and time data type

### String representation of date and time

The following formats are used to set the date and time:

| Type        | Default Format                                                                                                                                                     |
| :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `DATE`      | `YYYY-MM-DD`                                                                                                                                                       |
| `DATETIME`  | `YYYY-MM-DD hh:mm:ss`                                                                                                                                              |
| `TIMESTAMP` | `YYYY-MM-DD hh:mm:ss`                                                                                                                                              |
| `TIME`      | `hhh:mm:sss`                                                                                                                                                       |
| `YEAR`      | `YYYY` - full format <br /> `YY` or `Y` - shorthand format that returns a year between 2000-2069 for values ​​0-69 and a year between 1970-1999 for values ​​70-99 |

Moreover, when specifying a date, it is allowed to use any punctuation mark as a separator between parts of the date or time sections.
It is also possible to set the date without a separator character at all, together.

Examples of valid setting of values ​​for date and time via string representation:

```sql
SELECT  CAST("2022-06-16 16:37:23" AS DATETIME) AS datetime_1,
        CAST("2014/02/22 16*37*22" AS DATETIME) AS datetime_2,
        CAST("20220616163723" AS DATETIME) AS datetime_3,
        CAST("2021-02-12" AS DATE) AS date_1,
        CAST("160:23:13" AS TIME) AS time_1,
        CAST("89" AS YEAR) AS year
```

| datetime_1               | datetime_2               | datetime_3               | date_1                   | time_1    | year |
| ------------------------ | ------------------------ | ------------------------ | ------------------------ | --------- | ---- |
| 2022-06-16T16:37:23.000Z | 2014-02-22T16:37:22.000Z | 2022-06-16T16:37:23.000Z | 2021-02-12T00:00:00.000Z | 160:23:13 | 1989 |

In the query above, the `CAST` function was used to force the string to be converted to a date and time.
It is needed if the server does not expect the date and time and, accordingly, does not automatically convert the string
to the correct type. We'll learn more about type conversion in <a href="https://sql-academy.org/guide/type-conversion-functions" target="_blank">"Type conversion functions, CAST"</a>.

### Date generation functions

If you need to get the date and time from a string that does not match any format that accepts
`CAST` function, you can use the built-in `STR_TO_DATE` function, which takes an arbitrary string containing a date and a format describing it.

```sql
SELECT STR_TO_DATE('November 13, 1998', '%M %d, %Y') AS date;
```

| date                     |
| ------------------------ |
| 1998-11-13T00:00:00.000Z |

For a more detailed description of the `STR_TO_DATE` function and its arguments, see <a href="https://sql-academy.org/handbook/str_to_date" target="_blank">in the reference</a>.

To generate the current date or time, there is no need to create a string for its subsequent conversion to a date,
because there are built-in functions for getting given values: `CURDATE`, `CURTIME` and `NOW`.

```sql
SELECT CURDATE(), CURTIME(), NOW();
```

## Date and Time Extraction Functions

Sometimes it is necessary to obtain information not about the full date, but about its specific part, for example,
about its month or year.

To do this, SQL has the following functions:

| Function                                                                       | Description                                                                  |
| :----------------------------------------------------------------------------- | :--------------------------------------------------------------------------- |
| <a href="https://sql-academy.org/handbook/year" target="_blank">`YEAR`</a>     | Returns the year for the specified date                                      |
| <a href="https://sql-academy.org/handbook/month" target="_blank">`MONTH`</a>   | Returns the numeric value of the month of the year (from 1 to 12) for a date |
| <a href="https://sql-academy.org/handbook/day" target="_blank">`DAY`</a>       | Returns the ordinal number of the day in the month (from 1 to 31)            |
| <a href="https://sql-academy.org/handbook/hour" target="_blank">`HOUR`</a>     | Returns the hour value (between 0 and 23) for the time                       |
| <a href="https://sql-academy.org/handbook/minute" target="_blank">`MINUTE`</a> | Returns the minutes value (from 0 to 59) for the time                        |

## The difference between DATETIME and TIMESTAMP

MySQL has very similar data types: `DATETIME` and `TIMESTAMP`. Both are aimed at storing the date and time.
But they have a number of differences that determine which of these data types is best to use when.

| Criteria  | `DATETIME`                                                 | `TIMESTAMP`                                                                                                                        |
| :-------- | :--------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| Range     | from `1000-01-01 00:00:00` <br /> to `9999-12-31 23:59:59` | from `1970-01-01 00:00:00` <br /> to `2038-01-19 03:14:07`                                                                         |
| Time zone | Ignored <br /> Displayed as the date was set               | Taken into account <br /> When making selections, it is displayed taking into account the current time zone of the database server |

## Time zones

Since people all over the world want noon to approximate the maximum rise of the Sun, there has never been a problem
use universal time and the world was divided into 24 time zones.

UTC (Coordinated Universal Time) is currently used as the time reference point. All other time zones can be described
number of hours offset from UTC. For example, the Moscow time zone can be described as UTC+3.

The time zone is one of the database server settings and can be set:

-   globally
-   for the current user
-   for the current user session

```sql
SET GLOBAL time_zone = '+03:00';    // globally
SET time_zone = '+03:00';           // for the current user
SET @@session.time_zone = '+03:00'; // for the current user session
```

Accordingly, when changing the time zone, all values ​​of the `TIMESTAMP` type will be displayed taking into account the current active time zone.

## Examples of tasks for date and time

I would like to pay special attention to the most popular tasks related to the temporary data type,
where mistakes are often made.

### Age determination

When setting the task to find the age of a person by the date of his birth, there is often a temptation 😈 to calculate
the difference between the current year and the person's year of birth:

```sql
SELECT YEAR(NOW()) - YEAR('2003-07-03 14:10:26');
```

The problem with this approach is that it does not take into account whether the person had a birthday this year or not yet.
That is, if at the time of the request it was already July 3rd (07-03), then the person celebrated his birthday and he is already 20 years old,
otherwise he is still 19 years old.
The difference in `YEAR` functions will be useless here - in both cases it will give 20 years.

If determining the age in terms of the difference in years is a non-working option, then you may want to find the age in terms of
the difference of days between two dates, then divide this difference by the number of days in a year and round down:

```sql
SELECT FLOOR(DATEDIFF(NOW(), '2003-07-03 14:10:26') / 365);
```

And this solution will be much more accurate than the previous one. But it will not be absolutely accurate due to the presence of leap years, when there are 366 days in a year.
Although the error in calculating the age for 1 person due to the presence of a leap year is quite small, in the calculations for determining, say,
average age among a certain list of people, the error can accumulate and distort the real values.

And how then is it correct to determine the age? There is a built-in function for this - <a href="https://sql-academy.org/handbook/timestampdiff" target="_blank">`TIMESTAMPDIFF`</a>,
which takes as its first argument the unit in which to return the difference between two time values.

```sql
SELECT TIMESTAMPDIFF(YEAR, '2003-07-03 14:10:26', NOW());
```
