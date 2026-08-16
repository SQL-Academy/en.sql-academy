---
meta:
    title: "Numeric data type in SQL: MySQL and PostgreSQL"
    description: "Working with numbers in MySQL and PostgreSQL. Basic numeric data types: INTEGER, DECIMAL, FLOAT and others."
---

# Numeric data type

Numerical data are divided into exact and approximate, integer and real. Bit values are a separate category.

**MySQL**

## Exact integers

| Type                                    | Memory size | Range                                                                                                               |
| :-------------------------------------- | :---------- | :------------------------------------------------------------------------------------------------------------------ |
| `TINYINT`                               | 1 byte      | from -128 to 127 (from -2<sup>7</sup> to 2<sup>7</sup>-1) <br /> from 0 to 255 (from 0 to 2<sup>8</sup>-1)          |
| `SMALLINT`                              | 2 bytes     | from -32768 to 32767 (from -2<sup>15</sup> to 2<sup>15</sup>-1) <br /> from 0 to 65535 (from 0 to 2<sup>16</sup>-1) |
| `MEDIUMINT`                             | 3 bytes     | from -2<sup>23</sup> to 2<sup>23</sup>-1 <br /> from 0 to 2<sup>24</sup>-1                                          |
| `INT` <br /> `INTEGER` <br /> (aliases) | 4 bytes     | from -2<sup>31</sup> to 2<sup>31</sup>-1 <br /> from 0 to 2<sup>32</sup>-1                                          |
| `BIGINT`                                | 8 bytes     | from -2<sup>63</sup> to 2<sup>63</sup>-1 <br /> from 0 to 2<sup>64</sup>-1                                          |

Integers can be declared with the `UNSIGNED` keyword. In that case the column can no longer hold negative values, and its valid range doubles.
So `TINYINT` accepts values from -128 to 127, while `TINYINT UNSIGNED` accepts values from 0 to 255.

## Exact real numbers

| Type                                                  | Range                         |
| :---------------------------------------------------- | :---------------------------- |
| `DEC[(M,D)]` <br /> `DECIMAL[(M,D)]` <br /> (aliases) | Depends on M and D parameters |

The `DECIMAL` type stores exact real numbers. It is used when accuracy is critical — for example, when storing financial data.

Usage example:

```sql
CREATE TABLE Users (
    ...
    salary DECIMAL(5,2)
);
```

This example declares that the `salary` column stores numbers with a maximum of 5 digits, 2 of which are reserved for the decimal part.
So the column holds values in the range from -999.99 to 999.99.

The `DECIMAL` syntax is equivalent to `DECIMAL(M)` and `DECIMAL(M,0)`. By default, the `M` parameter is 10.

The integer part and the fractional part are stored as two separate integers, which makes the memory footprint easy to calculate.
For `DECIMAL(5,2)`, the integer part has 3 digits and takes 2 bytes, while the fractional part has 2 digits and needs just 1 byte — 3 bytes in total.

## Bit numbers

| Type                                     | Memory size | Range                                           |
| :--------------------------------------- | :---------- | :---------------------------------------------- |
| `BIT[(M)]`                               | M bit       | From 1 to 64 bits, depending on the M parameter |
| `BOOL` <br /> `BOOLEAN` <br /> (aliases) | 1 bit       | Either 0 or 1                                   |

The `BIT(M)` type stores a bit sequence of a given length. By default, the length is 8 bits.
If the value assigned to such a column uses fewer than M bits, it is padded with zeros on the left.
For example, writing `b'101'` to a `BIT(6)` column ends up stored as `b'000101'`.

## Approximate numbers

| Type                                                    | Memory size | Range                                                                             |
| :------------------------------------------------------ | :---------- | :-------------------------------------------------------------------------------- |
| `FLOAT[(M, D)]`                                         | 4 bytes     | Minimum value ±1.17·10<sup>-39</sup> <br /> Maximum value ±3.4·10<sup>38</sup>    |
| `REAL[(M, D)]` <br /> `DOUBLE[(M, D)]` <br /> (aliases) | 8 bytes     | Minimum value ±2.22·10<sup>-308</sup> <br /> Maximum value ±1.79·10<sup>308</sup> |

Floating-point numeric types can also take the `UNSIGNED` attribute.
As with integer types, it prevents negative values in the column, but — unlike integer types —
the maximum range of column values stays the same.

**PostgreSQL**

## Integer numbers

| Type                                    | Memory size | Range                                            |
| :-------------------------------------- | :---------- | :----------------------------------------------- |
| `SMALLINT`                              | 2 bytes     | from -32768 to 32767                             |
| `INT` <br /> `INTEGER` <br /> (aliases) | 4 bytes     | from -2147483648 to 2147483647                   |
| `BIGINT`                                | 8 bytes     | from -9223372036854775808 to 9223372036854775807 |

## Auto-increment types

| Type          | Memory size | Range                         |
| :------------ | :---------- | :---------------------------- |
| `SMALLSERIAL` | 2 bytes     | from 1 to 32767               |
| `SERIAL`      | 4 bytes     | from 1 to 2147483647          |
| `BIGSERIAL`   | 8 bytes     | from 1 to 9223372036854775807 |

The `SERIAL` types are pseudo-types for creating auto-incrementing columns. `SERIAL` is equivalent to `INTEGER` with an automatically created sequence.

## Exact real numbers

| Type                                                                                | Precision    | Range                                                                 |
| :---------------------------------------------------------------------------------- | :----------- | :-------------------------------------------------------------------- |
| `DECIMAL[(precision, scale)]` <br /> `NUMERIC[(precision, scale)]` <br /> (aliases) | User-defined | Up to 131072 digits before decimal point and up to 16383 digits after |

The `NUMERIC` type stores exact real numbers. It is used when accuracy is critical — for example, when storing financial data.

Usage example:

```sql
CREATE TABLE Users (
    ...
    salary NUMERIC(10,2)
);
```

This example declares that the `salary` column stores numbers with a maximum of 10 digits, 2 of which are reserved for the decimal part.
So the column holds values in the range from -99999999.99 to 99999999.99.

## Approximate numbers

| Type               | Memory size | Precision | Range                 |
| :----------------- | :---------- | :-------- | :-------------------- |
| `REAL`             | 4 bytes     | 6 digits  | from 1E-37 to 1E+37   |
| `DOUBLE PRECISION` | 8 bytes     | 15 digits | from 1E-307 to 1E+308 |

Floating point types are used for approximate calculations. PostgreSQL also supports special values: `Infinity`, `-Infinity` and `NaN` (not a number).
