---
meta:
  title: "Numeric data type in SQL: mathematical functions, rounding, ROUND, TRUNCATE"
  description: "Working with numbers in SQL: mathematical functions ROUND, TRUNCATE, CEILING, FLOOR, ABS, SIGN. Number rounding, precision control in MySQL and PostgreSQL."
---

# Numeric data type in SQL

Creating numeric data in SQL is quite simple: you can enter a number as a literal, you can get it from a table column, or
generate it by calculation.

When calculating, you can use all standard arithmetic operations (`+`, `-`, `*`, `/` and others) and change the priorities of calculations using brackets.

```sql-executable
SELECT 2 * ((22 - 16) / (2 + 1)) AS calc_example;
```

## Math functions

For most mathematical calculations, such as getting the power of a number or getting the square root, in SQL
there are built-in numeric functions. Here are some examples of these functions:

<MySQLOnly>

| Function name                                                       | Description                                                |
| :------------------------------------------------------------------ | ---------------------------------------------------------- |
| <a href="/handbook/mysql/pow" target="_blank">`POW(num, power)`</a> | Calculates a number to the specified power                 |
| <a href="/handbook/mysql/sqrt" target="_blank">`SQRT(num)`</a>      | Calculates the square root of a number                     |
| <a href="/handbook/mysql/log" target="_blank">`LOG(base, num)`</a>  | Calculates the logarithm of a number to the specified base |
| <a href="/handbook/mysql/exp" target="_blank">`EXP(num)`</a>        | Calculates e<sup>num</sup>                                 |
| <a href="/handbook/mysql/sin" target="_blank">`SIN(num)`</a>        | Calculates the sine of a number                            |
| <a href="/handbook/mysql/cos" target="_blank">`COS(num)`</a>        | Calculates the cosine of a number                          |
| <a href="/handbook/mysql/tan" target="_blank">`TAN(num)`</a>        | Calculates the tangent of a number                         |

A list of all numeric functions, their descriptions and examples can be found in the <a href="/handbook/mysql/ceiling" target="_blank">handbook</a>.

</MySQLOnly>

<PostgreSQLOnly>

| Function name                                                                | Description                                                |
| :--------------------------------------------------------------------------- | ---------------------------------------------------------- |
| <a href="/handbook/postgresql/power" target="_blank">`POWER(num, power)`</a> | Calculates a number to the specified power                 |
| <a href="/handbook/postgresql/sqrt" target="_blank">`SQRT(num)`</a>          | Calculates the square root of a number                     |
| <a href="/handbook/postgresql/log" target="_blank">`LOG(base, num)`</a>      | Calculates the logarithm of a number to the specified base |
| <a href="/handbook/postgresql/exp" target="_blank">`EXP(num)`</a>            | Calculates e<sup>num</sup>                                 |
| <a href="/handbook/postgresql/sin" target="_blank">`SIN(num)`</a>            | Calculates the sine of a number                            |
| <a href="/handbook/postgresql/cos" target="_blank">`COS(num)`</a>            | Calculates the cosine of a number                          |
| <a href="/handbook/postgresql/tan" target="_blank">`TAN(num)`</a>            | Calculates the tangent of a number                         |

A list of all numeric functions, their descriptions and examples can be found in the <a href="/handbook/postgresql/ceil" target="_blank">handbook</a>.

</PostgreSQLOnly>

## Round numbers

When working with floating point numbers, it is not always necessary to store or display numbers with full precision.
So, monetary transactions can be stored with an accuracy of up to 6 decimal places, and displayed up to 2, with an accuracy of kopecks.

<MySQLOnly>

SQL provides the following 4 functions for rounding numeric data: `CEIL`, `FLOOR`, `ROUND`,
`TRUNCATE`.

The functions `CEIL`, `FLOOR` are aimed at rounding a number to the nearest integer up and down, respectively.

```sql-executable
SELECT CEILING(69.69) AS ceiling, FLOOR(69.69) AS floor;
```

</MySQLOnly>

<PostgreSQLOnly>

SQL provides the following 4 functions for rounding numeric data: `CEIL`, `FLOOR`, `ROUND`,
`TRUNC`.

The functions `CEIL`, `FLOOR` are aimed at rounding a number to the nearest integer up and down, respectively.

```sql-executable
SELECT CEIL(69.69) AS ceiling, FLOOR(69.69) AS floor;
```

</PostgreSQLOnly>

To round to the nearest integer, there is a `ROUND` function, which rounds any number whose decimal part is greater than or equal to 0.5.
side, otherwise less.

```sql-executable
SELECT ROUND(69.499), ROUND(69.5), ROUND(69.501);
```

The `ROUND` function also allows you to round a number to some fraction of decimal places.
To do this, the function takes an optional second argument indicating the number of decimal places to leave.

```sql-executable
SELECT ROUND(69.7171,1), ROUND(69.7171,2), ROUND(69.7171,3);
```

The second argument to the `ROUND` function can also take negative values.
In this case, the digits to the left of the decimal point of the number become equal to zero by the number specified in the argument, and the fractional part is cut off.

```sql-executable
SELECT ROUND(1691.7,-1), ROUND(1691.7,-2), ROUND(1691.7,-3);
```

<MySQLOnly>

The `TRUNCATE` function is similar to the `ROUND` function, it is also capable of taking an optional 2nd parameter, only instead of rounding it simply
discards unnecessary numbers.

```sql-executable
SELECT TRUNCATE(69.7979,1), TRUNCATE(69.7979,2), TRUNCATE(69.7979,3);
```

What will the following expression return?

```sql-executable
SELECT TRUNCATE(69.7979, -1);
```

</MySQLOnly>

<PostgreSQLOnly>

The `TRUNC` function is similar to the `ROUND` function, it is also capable of taking an optional 2nd parameter, only instead of rounding it simply
discards unnecessary numbers.

```sql-executable
SELECT TRUNC(69.7979,1), TRUNC(69.7979,2), TRUNC(69.7979,3);
```

What will the following expression return?

```sql-executable
SELECT TRUNC(69.7979, -1);
```

</PostgreSQLOnly>

## Working with signed numbers

When working with numeric data that may contain negative values, the `SIGN` and `ABS` functions can be useful.

The `SIGN` function returns `-1` if the number is negative, `0` if the number is zero, and `1` if the number is positive.

```sql-executable
SELECT SIGN(-69), SIGN(0), SIGN(69);
```

The `ABS` function returns the absolute value of a number.

```sql-executable
SELECT ABS(-69), ABS(0), ABS(69);
```
