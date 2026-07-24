---
meta:
    title: "Type conversion functions, CAST: MySQL and PostgreSQL"
    description: "SQL type conversion: CAST function in MySQL and PostgreSQL, CONVERT in MySQL. Data types for conversion, syntax and examples."
---

# Type conversion functions, CAST

When we perform operations on values ​​with different data types, the DBMS tries to perform a conversion and cast the values ​​used to the desired type.
For example, in the example below we are comparing values ​​with `STRING` and `INT` types. To perform this comparison, the DBMS automatically
will convert a string value to a numeric value.

**MySQL**

```sql
SELECT '50' > 49 AS comparison_1, '50' > 51 AS comparison_2;
```

| comparison_1 | comparison_2 |
| ------------ | ------------ |
| 1            | 0            |

**PostgreSQL**

```sql
SELECT '50' > 49 AS comparison_1, '50' > 51 AS comparison_2;
```

| comparison_1 | comparison_2 |
| ------------ | ------------ |
| true         | false        |

But not all DBMS conversions can be done automatically, and then it is necessary to do an explicit type conversion.

**MySQL**

To do this, MySQL has two very similar functions `CAST` and `CONVERT`.

**PostgreSQL**

To do this, PostgreSQL has the `CAST` function and the `::` operator.

## Syntax

**MySQL**

```sql
CAST(value AS conversion_type);
CONVERT(value, conversion_type);
```

Example,

```sql
SELECT CAST(12005.6 AS DECIMAL) AS cast_example, CONVERT(12005.4, DECIMAL) AS convert_example;
```

| cast_example | convert_example |
| ------------ | --------------- |
| 12006        | 12005           |

**PostgreSQL**

```sql
CAST(value AS conversion_type);
value::conversion_type;
```

Example,

```sql
SELECT CAST(12005.6 AS INTEGER) AS cast_example, 12005.4::INTEGER AS operator_example;
```

| cast_example | operator_example |
| ------------ | ---------------- |
| 12006        | 12005            |

**MySQL**

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

**PostgreSQL**

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

## Impossibility of any conversion

Using the `CAST` function imposes requirements on the format of the original value. And the question immediately arises,
what happens if the given format does not match the required one?
For example, if you try to convert random text to a temporal data type:

**MySQL**

```sql
SELECT CAST('SQL Academy' AS DATETIME) AS invalid_cast;
```

| invalid_cast |
| ------------ |
| \<NULL>      |

In this case, MySQL will return `NULL` instead of the converted value.

**PostgreSQL**

```sql
SELECT CAST('SQL Academy' AS TIMESTAMP) AS invalid_cast;
```

In this case, PostgreSQL will return an error, as the string cannot be converted to a date.

## Self test

So, what is the responsibility of the `CAST` function in SQL 🧐?

1. The function is used when it is necessary to determine the data type of the passed value. — The CAST function is responsible for converting a value, not for determining its data type.

2. **Correct answer:&#x20;**&#x54;he function is responsible for converting a value from one data type to another — The function is really responsible for the explicit type conversion.

3. The function is required to perform mathematical calculations — Perhaps you should revisit this lesson. The CAST function is responsible for type conversion, not for mathematical calculations.
