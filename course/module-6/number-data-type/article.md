# Numeric data type

Numerical data are divided into exact and approximate, integer and real. Bit values are a separate category.

## Exact integers

| Type                                    | Memory size | Range                                                                                                               |
| :-------------------------------------- | :---------- | :------------------------------------------------------------------------------------------------------------------ |
| `TINYINT`                               | 1 byte      | from -128 to 127 (from -2<sup>7</sup> to 2<sup>7</sup>-1) <br /> from 0 to 255 (from 0 to 2<sup>8</sup>-1)          |
| `SMALLINT`                              | 2 byte      | from -32768 to 32767 (from -2<sup>15</sup> to 2<sup>15</sup>-1) <br /> from 0 to 65535 (from 0 to 2<sup>16</sup>-1) |
| `MEDIUMINT`                             | 3 byte      | from -2<sup>23</sup> to 2<sup>23</sup>-1 <br /> from 0 to 2<sup>24</sup>-1                                          |
| `INT` <br /> `INTEGER` <br /> (aliases) | 4 byte      | from -2<sup>31</sup> to 2<sup>31</sup>-1 <br /> from 0 to 2<sup>32</sup>-1                                          |
| `BIGINT`                                | 8 byte      | from -2<sup>63</sup> to 2<sup>63</sup>-1 <br /> from 0 to 2<sup>64</sup>-1                                          |

Integers can be declared with the keyword `UNSIGNED`. In this case, the elements of this column it will not be possible to assign negative values, and the valid range,
which takes on the type doubles. So, type `TINYINT` can take values from -128 to 127, and `TINYINT UNSIGNED` — from 0 to 255.

## Exact real numbers

| Type                                                  | Range                         |
| :---------------------------------------------------- | :---------------------------- |
| `DEC[(M,D)]` <br /> `DECIMAL[(M,D)]` <br /> (aliases) | Depends on M and D parameters |

Type `DECIMAL` stores exact real values data. It is used when accuracy is critical. For example, when storing financial data.

Usage example:

```sql
CREATE TABLE Users (
    ...
    salary DECIMAL(5,2)
);
```

This example declares that the `salary` column will store numbers, having a maximum of 5 digits, 2 of which are reserved for decimal part.
That is, this column will store values in the range from -999.99 to 999.99.

The syntax `DECIMAL` is equivalent to `DECIMAL(M)` and `DECIMAL(M,0)`. By default, the parameter `M` is 10.

The whole part and the part after the point are stored as 2 separate integers. On Based on this fact,
the amount of memory consumed can be easily calculated. So for `DECIMAL(5,2)` the integer part contains
3 digits and takes 2 byte, part after the point have 2 digits - 1 byte is enough. Total for storage will be spent 3 byte.

## Bit numbers

| Type                                     | Memory size | Range                                       |
| :--------------------------------------- | :---------- | :------------------------------------------ |
| `BIT[(M)]`                               | M bit       | From 1 to 64 bit, depend on the M parameter |
| `BOOL` <br /> `BOOLEAN` <br /> (aliases) | 1 bit       | Or 0, or 1                                  |

Type `BIT(M)` stores a sequence of bits a given length. By default, the length is 8 bit.
If assigned the value in a column of this type uses less than M bit, then zeros are padded on the left.
For example, when trying to write the value `b'101'` to `BIT(6)` will eventually be stored `b'000101'`.

## Approximate numbers

| Type                                                    | Memory size | Range                                                                             |
| :------------------------------------------------------ | :---------- | :-------------------------------------------------------------------------------- |
| `FLOAT[(M, D)]`                                         | 4 byte      | Minimum value ±1.17·10<sup>-39</sup> <br /> Maximum value ±3.4·10<sup>38</sup>    |
| `REAL[(M, D)]` <br /> `DOUBLE[(M, D)]` <br /> (aliases) | 8 byte      | Minimum value ±2.22·10<sup>-308</sup> <br /> Maximum value ±1.79·10<sup>308</sup> |

Numeric floating point data types can also have a parameter `UNSIGNED`.
As with integer types, this attribute prevents negative storage in the marked column values, but,
unlike integer types, the maximum interval for column values remains the same.
