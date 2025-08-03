---
meta:
  title: "Literals in SQL"
  description: 'Literal — an explicitly specified fixed value, for example, the number 12 or the string "SQL". In MySQL, there are the following types of literals: numeric, string, boolean, NULL, bit, hexadecimal, and date and time literal.'
---

# Literals in SQL

In the last lesson, a string was output, and if we speak in a more formal language, then the so-called string literal.

```sql-executable
SELECT 'Hello world'
```

> A literal is an explicitly specified fixed value, such as the number `12` or the string `'SQL'`.

The main types of literals in SQL are:

- string
- numeric
- logical
- NULL
- date and time literal

## String literals

<MySQLOnly>

A string is a sequence of characters enclosed in single (') or double (") quotation marks.
For example, `'this is a string'` and `"this is a string"`.

</MySQLOnly>

<PostgreSQLOnly>

A string is a sequence of characters enclosed in single quotation marks (').
For example, `'this is a string'`.

In PostgreSQL, double quotes (") are used for identifiers (table names, column names), and cannot be used for string literals.

</PostgreSQLOnly>

Strings can contain special sequences of characters starting with `"\"` (escape character).
They are needed in order for the DBMS to give ordinary symbols (letters and other signs) a new special meaning. For example, the sequence `"\n"`
literally means "new line", and without the preceding slash it would be the usual letter `"n"`.

- ```sql-Family-executable
  SELECT 'Line Another line' as String
  ```

- ```sql-Family-executable
  SELECT 'Line \n Another line' as String
  ```

## Numeric literals

|                                                                                                               | Example                       |
| :------------------------------------------------------------------------------------------------------------ | :---------------------------- |
| Includes integers and floating-point numbers. The decimal separator for a floating-point number is "." (dot). | `1`, `2.9`, `0.01`            |
| Can have only an integer part, a fractional part, or both.                                                    | `.2`, `1.1`, `10`             |
| Can be positive or negative (it's not necessary to specify a sign for a positive number).                     | `+1`, `-10`, `-2.2`           |
| Can be represented in exponential notation.                                                                   | `1e3` (=1000) `1e-3` (=0.001) |

### Arithmetic operators

For numeric literals, SQL has all the arithmetic operators we are familiar with:

<MySQLOnly>

|  Operator  | Description      | Example         |
| :--------: | :--------------- | :-------------- |
| `%`, `MOD` | Modulus division | `11 % 5 = 1`    |
|    `*`     | Multiplication   | `10 * 16 = 160` |
|    `+`     | Addition         | `98 + 2 = 100`  |
|    `-`     | Subtraction      | `50 - 51 = -1`  |
|    `/`     | Division         | `1 / 2 = 0.5`   |
|   `DIV`    | Integer division | `10 DIV 4 = 2`  |

</MySQLOnly>

<PostgreSQLOnly>

| Operator | Description      | Example         |
| :------: | :--------------- | :-------------- |
|   `%`    | Modulus division | `11 % 5 = 1`    |
|   `*`    | Multiplication   | `10 * 16 = 160` |
|   `+`    | Addition         | `98 + 2 = 100`  |
|   `-`    | Subtraction      | `50 - 51 = -1`  |
|   `/`    | Division         | `1 / 2 = 0.5`   |

</PostgreSQLOnly>

Using these operators, you can construct any arithmetic expression by applying the standard rules of arithmetic.

For example:

```sql-Family-executable
SELECT (5 * 2 - 6) / 2 AS Result;
```

## Date and Time Literals

Date and time values can be represented as strings or numbers.

<MySQLOnly>

For example, if we want to specify a date in a query, we can do this using the string `"1970-12-30"`, `"19701230"`, or the number `19701230`.
In both cases, these values will be interpreted as the date "December 30th, 1970".

</MySQLOnly>

<PostgreSQLOnly>

For example, if we want to specify a date in a query, we can do this using the string `'1970-12-30'`.

</PostgreSQLOnly>

Here is an example of using a date literal:

```sql-Family-executable
SELECT * FROM FamilyMembers WHERE birthday > '1970-12-30'
```

You don't need to pay attention to what this query specifically does, we'll look at it later, but right now it's important to focus on the syntax of how we can specify a date in a query.

Above, we looked at how to specify a date, but in addition to a date, we can also specify a time or both together.

<MySQLOnly>

|               | Description                                          | Format                                                                                                                                                                             |
| :------------ | :--------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Date          | Interpreted as a date with zero time                 | `YYYY-MM-DD`, `YYYYMMDD` <br /><br /> Any punctuation mark can be used instead of the "-" separator. <br /><br /> For example: <br /> `'2020-01-01'` = January 1st, 2020, 00:00:00 |
| Time          | Contains only time without a specific date           | `hh:mm:ss`, `hh:mm`, `hh`, `ss` <br /><br /> The separator can also be omitted. <br /><br /> For example: <br /> `12:11` = 12:11:00                                                |
| Date and time | Date with the possibility of setting a specific time | `YYYY-MM-DD hh:mm:ss`, `YYYYMMDDhhmmss` <br /><br /> For example: <br /> `'20200101183030'` = January 1st, 2020, 18:30:30                                                          |

</MySQLOnly>

<PostgreSQLOnly>

|               | Description                                          | Format                                                                                                       |
| :------------ | :--------------------------------------------------- | :----------------------------------------------------------------------------------------------------------- |
| Date          | Interpreted as a date with zero time                 | `YYYY-MM-DD` <br /><br /> For example: <br /> `'2020-01-01'` = January 1st, 2020, 00:00:00                   |
| Time          | Contains only time without a specific date           | `hh:mm:ss`, `hh:mm` <br /><br /> For example: <br /> `'12:11:00'`, `'12:11'`                                 |
| Date and time | Date with the possibility of setting a specific time | `YYYY-MM-DD hh:mm:ss` <br /><br /> For example: <br /> `'2020-01-01 18:30:30'` = January 1st, 2020, 18:30:30 |

</PostgreSQLOnly>

## Logical literals

A logical literal is a value of `TRUE` or `FALSE`, which indicates the truthfulness or falsehood of a statement.

<MySQLOnly>
    When interpreting a query, MySQL converts them into numbers: `TRUE` and `FALSE` become
    `1` and `0`, respectively.
</MySQLOnly>

## NULL

The value `NULL` means "no data" or "no value." It is used to visually distinguish between empty values, such as a zero-length string or a space, and the absence of a value altogether.
