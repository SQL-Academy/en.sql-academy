---
meta:
  title: "Type conversion functions, CAST: MySQL and PostgreSQL"
  description: "SQL type conversion: CAST function in MySQL and PostgreSQL, CONVERT in MySQL. Data types for conversion, syntax and examples."
---

# Type conversion functions, CAST

When we perform operations on values ​​with different data types, the DBMS tries to perform a conversion and cast the values ​​used to the desired type.
For example, in the example below we are comparing values ​​with `STRING` and `INT` types. To perform this comparison, the DBMS automatically
will convert a string value to a numeric value.

<MySQLOnly>

```sql-executable
SELECT '50' > 49 AS comparison_1, '50' > 51 AS comparison_2;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
SELECT '50' > 49 AS comparison_1, '50' > 51 AS comparison_2;
```

</PostgreSQLOnly>

But not all DBMS conversions can be done automatically, and then it is necessary to do an explicit type conversion.

<MySQLOnly>

To do this, MySQL has two very similar functions `CAST` and `CONVERT`.

</MySQLOnly>

<PostgreSQLOnly>

To do this, PostgreSQL has the `CAST` function and the `::` operator.

</PostgreSQLOnly>

## Syntax

<MySQLOnly>

```sql
CAST(value AS conversion_type);
CONVERT(value, conversion_type);
```

Example,

```sql-executable
SELECT CAST(12005.6 AS DECIMAL) AS cast_example, CONVERT(12005.4, DECIMAL) AS convert_example;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
CAST(value AS conversion_type);
value::conversion_type;
```

Example,

```sql-executable
SELECT CAST(12005.6 AS NUMERIC) AS cast_example, 12005.4::NUMERIC AS operator_example;
```

</PostgreSQLOnly>

<MySQLOnly>

The CAST function can convert the passed value to any of the following types:

| Type               | Description                                                                                                                                                                                                   |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `DATE`             | Converts a value to `DATE`. Format: "YYYY-MM-DD".                                                                                                                                                             |
| `DATETIME`         | Converts a value to `DATETIME`. Format: "YYYY-MM-DD hh:mm:ss".                                                                                                                                                |
| `TIME`             | Converts a value to `TIME`. Format: "hh:mm:ss".                                                                                                                                                               |
| `DECIMAL[(M[,D])]` | Converts a value to `DECIMAL`. It has two optional arguments `M` and `D`, which define the maximum number of characters before and after the decimal point, respectively. By default, `D` is 0 and `M` is 10. |
| `CHAR[(N)]`        | Converts value to `CHAR`. You can pass the maximum length of the string as an optional argument.                                                                                                              |
| `SIGNED`           | Converts a value to a `BIGINT` value.                                                                                                                                                                         |
| `UNSIGNED`         | Converts a value to an unsigned `BIGINT` value.                                                                                                                                                               |
| `BINARY`           | Converts a value to `BINARY`.                                                                                                                                                                                 |
| `YEAR`             | Converts a value to a year.                                                                                                                                                                                   |

</MySQLOnly>

<PostgreSQLOnly>

The CAST function can convert the passed value to any of the following types:

| Type               | Description                                                                                                                                                               |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `DATE`             | Converts a value to `DATE`. Format: "YYYY-MM-DD".                                                                                                                         |
| `TIMESTAMP`        | Converts a value to `TIMESTAMP`. Format: "YYYY-MM-DD hh:mm:ss".                                                                                                           |
| `TIME`             | Converts a value to `TIME`. Format: "hh:mm:ss".                                                                                                                           |
| `NUMERIC[(M[,D])]` | Converts a value to `NUMERIC`. It has two optional arguments `M` and `D`, which define the maximum number of characters before and after the decimal point, respectively. |
| `VARCHAR[(N)]`     | Converts value to `VARCHAR`. You can pass the maximum length of the string as an optional argument.                                                                       |
| `INTEGER`          | Converts a value to an integer.                                                                                                                                           |
| `BIGINT`           | Converts a value to a big integer.                                                                                                                                        |
| `BOOLEAN`          | Converts a value to a boolean type.                                                                                                                                       |
| `TEXT`             | Converts a value to a text type.                                                                                                                                          |

</PostgreSQLOnly>

## Impossibility of any conversion

Using the `CAST` function imposes requirements on the format of the original value. And the question immediately arises,
what happens if the given format does not match the required one?
For example, if you try to convert random text to a temporal data type:

<MySQLOnly>

```sql-executable
SELECT CAST('SQL Academy' AS DATETIME) AS invalid_cast;
```

In this case, MySQL will return `NULL` instead of the converted value.

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable
SELECT CAST('SQL Academy' AS TIMESTAMP) AS invalid_cast;
```

In this case, PostgreSQL will return an error, as the string cannot be converted to a date.

</PostgreSQLOnly>

## Self test

So, what is the responsibility of the `CAST` function in SQL 🧐?
