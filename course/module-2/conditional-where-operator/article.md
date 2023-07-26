# Conditional operator WHERE

The situation where a selection needs to be made based on a specific condition is very common.
For this, the `WHERE` operator exists in the `SELECT` statement, which is followed by conditions for limiting rows.
If a record satisfies this condition, it appears in the result; otherwise, it is discarded.

## General structure of a query with the WHERE operator

```sql
SELECT [DISTINCT] table_fields FROM table_name
WHERE row_limit_conditions
[logical_operator other_row_limit_conditions];
```

For example, a query using the `WHERE` operator may look like this:

```sql
SELECT * FROM Student
WHERE first_name = "Grigorij" AND YEAR(birthday) > 2000;
```

| id  | first_name | middle_name | last_name | birthday                 | address                         |
| --- | ---------- | ----------- | --------- | ------------------------ | ------------------------------- |
| 33  | Grigorij   | Gennadevich | Kapustin  | 2001-12-13T00:00:00.000Z | ul. Pervomajskaya, d. 45, kv. 6 |
| 65  | Grigorij   | Kirillovich | Kolobov   | 2003-07-17T00:00:00.000Z | ul. CHernova, d. 9, kv. 34      |

This code snippet uses:

- two comparison operators
  `first_name = "Grigorij"` and `YEAR(birthday) > 2000;`
- one logical operator `AND`

As a result, we obtain data on students whose first name is "Grigorij" and whose birth year is greater than 2000.

## Comparison operators

Comparison operators are used to compare 2 expressions, the result of which can be:

- `true` (equivalent to 1)
- `false` (equivalent to 0)
- `NULL`

| Operator              | Sign         | Description                                                                                                                                                     |
| :-------------------- | :----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Equality              | `=`          | If both values are equal, then the result will be 1, otherwise 0                                                                                                |
| Equivalence           | `<=>`        | It is similar to the equality operator, except that the result will be 1 in the case of comparing `NULL` with `NULL` and 0 when comparing any value with `NULL` |
| Inequality            | `<>` or `!=` | If both values are not equal, then the result will be 1, otherwise 0                                                                                            |
| Less than             | `<`          | If one value is less than the other, the result will be 1, otherwise 0                                                                                          |
| Less than or equal    | `<=`         | If one value is less than or equal to the other, the result will be 1, otherwise 0                                                                              |
| Greater than          | `>`          | If one value is greater than the other, the result will be 1, otherwise 0                                                                                       |
| Greater than or equal | `>=`         | If one value is greater than or equal to the other, the result will be 1, otherwise 0                                                                           |

> The result of comparison with `NULL` is `NULL`. The exception is the equivalence operator.

Try to run the query and play with these operators in the sandbox yourself:

```sql
SELECT
    2 = 1,
	'a' = 'a',
	NULL <=> NULL,
	2 <> 2,
	3 < 4,
	10 <= 10,
	7 > 1,
	8 >= 10;
```

| 2 = 1 | 'a' = 'a' | 1 <=> NULL | NULL <=> NULL | 2 <> 2 | 3 < 4 | 10 <= 10 | 7 > 1 | 8 >= 10 |
| ----- | --------- | ---------- | ------------- | ------ | ----- | -------- | ----- | ------- |
| 0     | 1         | 0          | 1             | 0      | 1     | 1        | 1     | 0       |

## Logical Operators

Logical operators are necessary for linking comparison operators.

| Operator | Description                                                                                |
| :------- | :----------------------------------------------------------------------------------------- |
| `NOT`    | Changes the value of the comparison operator to the opposite                               |
| `OR`     | Returns the common value of the expression to be true if at least one of them is true      |
| `AND`    | Returns the common value of the expression to be true if they are both true                |
| `XOR`    | Returns the common value of the expression to be true if one and only one argument is true |

Let's take for example, outputting all flights that were made on a "Boeing" plane, but the departure was not from London:

```sql
SELECT * FROM Trip
WHERE plane = 'Boeing' AND NOT town_from = 'London';
```
