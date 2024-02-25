---
meta:
    title: 'Type conversion functions, CAST'
    description: 'SQL type conversion, using the CAST and CONVERT functions'
---

# Type conversion functions, CAST

When we perform operations on values ​​with different data types, the DBMS tries to perform a conversion and cast the values ​​used to the desired type.
For example, in the example below we are comparing values ​​with `STRING` and `INT` types. To perform this MySQL comparison automatically
will convert a string value to a numeric value.

```sql
SELECT '50' > 49, '50' > 51;
```

| '50' > 49 | '50' > 51 |
| --------- | --------- |
| 1         | 0         |

But not all DBMS conversions can be done automatically, and then it is necessary to do an explicit type conversion.
To do this, MySQL has two very similar functions `CAST` and `CONVERT`.

## Syntax

```sql
CAST(value AS conversion_type);
CONVERT(value, conversion_type);
```

Example,

```sql
SELECT CAST(12005.6 AS DECIMAL), CONVERT(12005.4, DECIMAL);
```

| 'CAST(12005.6 AS DECIMAL)' | 'CONVERT(12005.4, DECIMAL)' |
| -------------------------- | --------------------------- |
| 12006                      | 12005                       |

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

## Impossibility of any conversion

Using the `CAST` function imposes requirements on the format of the original value. And the question immediately arises,
what happens if the given format does not match the required one?
For example, if you try to convert random text to a temporary data type:

```sql
SELECT CAST('SQL Academy' AS DATETIME);
```

| CAST('SQL Academy' AS DATETIME) |
| ------------------------------- |
| <NULL>                          |

In this case, MySQL will return `NULL` instead of the converted value.
