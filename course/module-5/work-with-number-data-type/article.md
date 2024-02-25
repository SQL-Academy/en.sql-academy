---
meta:
    title: 'Numeric data type in SQL'
    description: 'Working with numbers in SQL. Mathematical functions, control of the precision of numbers in SQL, as well as working with signed data.'
---

# Numeric data type in SQL

Creating numeric data in SQL is quite simple: you can enter a number as a literal, you can get it from a table column, or
generate it by calculation.

When calculating, you can use all standard arithmetic operations (`+`, `-`, `*`, `/` and others) and change the priorities of calculations using brackets.

```sql
SELECT 2 * ((22 - 16) / (2 + 1)) AS calc_example;
```

| calc_example |
| ------------ |
| 4            |

## Math functions

For most mathematical calculations, such as getting the power of a number or getting the square root, in SQL
there are built-in numeric functions. Here are some examples of these functions:

| Function name                                                                        | Description                                                |
| :----------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| <a href="https://sql-academy.org/handbook/pow" target="_blank">`POW(num, power)`</a> | Calculates a number to the specified power                 |
| <a href="https://sql-academy.org/handbook/sqrt" target="_blank">`SQRT(num)`</a>      | Calculates the square root of a number                     |
| <a href="https://sql-academy.org/handbook/log" target="_blank">`LOG(base, num)`</a>  | Calculates the logarithm of a number to the specified base |
| <a href="https://sql-academy.org/handbook/exp" target="_blank">`EXP(num)`</a>        | Calculates e<sup>num</sup>                                 |
| <a href="https://sql-academy.org/handbook/sin" target="_blank">`SIN(num)`</a>        | Calculates the sine of a number                            |
| <a href="https://sql-academy.org/handbook/cos" target="_blank">`COS(num)`</a>        | Calculates the cosine of a number                          |
| <a href="https://sql-academy.org/handbook/tan" target="_blank">`TAN(num)`</a>        | Calculates the tangent of a number                         |

A list of all numeric functions, their descriptions and examples can be found in the <a href="https://sql-academy.org/handbook/ceiling" target="_blank">handbook</a>.

## Round numbers

When working with floating point numbers, it is not always necessary to store or display numbers with full precision.
So, monetary transactions can be stored with an accuracy of up to 6 decimal places, and displayed up to 2, with an accuracy of kopecks.

SQL provides the following 4 functions for rounding numeric data: `CEIL`, `FLOOR`, `ROUND`,
`TRUNCATE`.

The functions `CEIL`, `FLOOR` are aimed at rounding a number to the nearest integer up and down, respectively.

```sql
SELECT CEILING(69.69) AS ceiling, FLOOR(69.69) AS floor;
```

| ceiling | floor |
| ------- | ----- |
| 70      | 69    |

To round to the nearest integer, there is a `ROUND` function, which rounds any number whose decimal part is greater than or equal to 0.5.
side, otherwise less.

```sql
SELECT ROUND(69.499), ROUND(69.5), ROUND(69.501);
```

| ROUND(69.499) | ROUND(69.5) | ROUND(69.501) |
| ------------- | ----------- | ------------- |
| 69            | 70          | 70            |

The `ROUND` function also allows you to round a number to some fraction of decimal places.
To do this, the function takes an optional second argument indicating the number of decimal places to leave.

```sql
SELECT ROUND(69.7171,1), ROUND(69.7171,2), ROUND(69.7171,3);
```

| ROUND(69.7171,1) | ROUND(69.7171,2) | ROUND(69.7171,3) |
| ---------------- | ---------------- | ---------------- |
| 69.7             | 69.72            | 69.717           |

The second argument to the `ROUND` function can also take negative values.
In this case, the digits to the left of the decimal point of the number become equal to zero by the number specified in the argument, and the fractional part is cut off.

```sql
SELECT ROUND(1691.7,-1), ROUND(1691.7,-2), ROUND(1691.7,-3);
```

| ROUND(1691.7,-1) | ROUND(1691.7,-2) | ROUND(1691.7,-3) |
| ---------------- | ---------------- | ---------------- |
| 1690             | 1700             | 2000             |

The `TRUNCATE` function is similar to the `ROUND` function, it is also capable of taking an optional 2nd parameter, only instead of rounding it simply
discards unnecessary numbers.

```sql
SELECT TRUNCATE(69.7979,1), TRUNCATE(69.7979,2), TRUNCATE(69.7979,3);
```

| TRUNCATE(69.7979,1) | TRUNCATE(69.7979,2) | TRUNCATE(69.7979,3) |
| ------------------- | ------------------- | ------------------- |
| 69.7                | 69.79               | 69.797              |

What will the following expression return?

```sql
SELECT TRUNCATE(69.7979, -1);
```

## Working with signed numbers

When working with numeric data that may contain negative values, the `SIGN` and `ABS` functions can be useful.

The `SIGN` function returns `-1` if the number is negative, `0` if the number is zero, and `1` if the number is positive.

```sql
SELECT SIGN(-69), SIGN(0), SIGN(69);
```

| SIGN(-69) | SIGN(0) | SIGN(69) |
| --------- | ------- | -------- |
| -1        | 0       | 1        |

The `ABS` function returns the absolute value of a number.

```sql
SELECT ABS(-69), ABS(0), ABS(69);
```

| ABS(-69) | ABS(0) | ABS(69) |
| -------- | ------ | ------- |
| 69       | 0      | 69      |
