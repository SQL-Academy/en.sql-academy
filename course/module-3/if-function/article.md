# Conditional IF function

In the previous lesson, we looked at the `CASE` statement for implementing conditional logic in SQL.
However, this is not the only mechanism by which it is possible to implement logic branching in a query.
It's time to turn our attention to the `IF` function.

## Syntax

```sql
IF (conditional_expression,
    value_under_true_condition,
    value_on_false_condition);
```

The `IF` function looks at the truth of the conditional expression and, depending on this truth, returns either
the value passed in the second argument or the third argument.

### Examples

- Simple example

  ```sql
  SELECT IF(10>20, "TRUE", "FALSE");
  ```

  | IF(10>20, "TRUE", "FALSE") |
  | -------------------------- |
  | FALSE                      |

- Example of use with a real database

  ```sql
  SELECT id, price,
      IF(price >= 150, "Comfort class", "Economy class") AS category
      FROM Rooms
  ```

  | id  | price | category      |
  | --- | ----- | ------------- |
  | 1   | 149   | Economy class |
  | 2   | 225   | Comfort class |
  | 3   | 150   | Comfort class |
  | 4   | 89    | Economy class |
  | 5   | 80    | Economy class |
  | 6   | 200   | Comfort class |
  | 7   | 60    | Economy class |
  | 8   | 79    | Economy class |
  | 9   | 79    | Economy class |
  | 10  | 150   | Comfort class |
  | 11  | 135   | Economy class |
  | 12  | 85    | Economy class |
  | 13  | 89    | Economy class |
  | 14  | 85    | Economy class |
  | 15  | 120   | Economy class |
  | 16  | 140   | Economy class |
  | 17  | 215   | Comfort class |
  | 18  | 140   | Economy class |
  | 19  | 99    | Economy class |
  | 20  | 190   | Comfort class |
  | 21  | 299   | Comfort class |
  | 22  | 130   | Economy class |
  | 23  | 80    | Economy class |
  | 24  | 110   | Economy class |
  | 25  | 120   | Economy class |
  | 26  | 60    | Economy class |
  | 27  | 80    | Economy class |
  | 28  | 150   | Comfort class |
  | 29  | 44    | Economy class |
  | 30  | 30    | Comfort class |
  | 31  | 31    | Economy class |
  | 32  | 32    | Economy class |
  | 33  | 33    | Economy class |
  | 34  | 34    | Economy class |
  | 35  | 35    | Economy class |
  | 36  | 36    | Economy class |
  | 37  | 37    | Economy class |
  | 38  | 38    | Economy class |
  | 39  | 39    | Comfort class |
  | 40  | 40    | Economy class |
  | 41  | 41    | Economy class |
  | 42  | 42    | Economy class |
  | 43  | 43    | Economy class |
  | 44  | 44    | Economy class |
  | 45  | 45    | Comfort class |
  | 46  | 46    | Comfort class |
  | 47  | 47    | Economy class |
  | 48  | 48    | Economy class |
  | 49  | 49    | Economy class |
  | 50  | 50    | Economy class |

- `IF` functions can also be nested

  ```sql
  SELECT id, price,
      IF(price >= 200, "Business Class",
          IF(price >= 150,
              "Comfort class", "Economy class")) AS category
      FROM Rooms
  ```

  | id  | price | category       |
  | --- | ----- | -------------- |
  | 1   | 1     | Economy class  |
  | 2   | 2     | Business Class |
  | 3   | 3     | Comfort class  |
  | 4   | 4     | Economy class  |
  | 5   | 5     | Economy class  |
  | 6   | 6     | Business Class |
  | 7   | 7     | Economy class  |
  | 8   | 8     | Economy class  |
  | 9   | 9     | Economy class  |
  | 10  | 10    | Comfort class  |
  | 11  | 11    | Economy class  |
  | 12  | 12    | Economy class  |
  | 13  | 13    | Economy class  |
  | 14  | 14    | Economy class  |
  | 15  | 15    | Economy class  |
  | 16  | 16    | Economy class  |
  | 17  | 17    | Business Class |
  | 18  | 18    | Economy class  |
  | 19  | 19    | Economy class  |
  | 20  | 20    | Comfort class  |
  | 21  | 21    | Business Class |
  | 22  | 22    | Economy class  |
  | 23  | 23    | Economy class  |
  | 24  | 24    | Economy class  |
  | 25  | 25    | Economy class  |
  | 26  | 26    | Economy class  |
  | 27  | 27    | Economy class  |
  | 28  | 28    | Comfort class  |
  | 29  | 29    | Economy class  |
  | 30  | 30    | Comfort class  |
  | 31  | 31    | Economy class  |
  | 32  | 32    | Economy class  |
  | 33  | 33    | Economy class  |
  | 34  | 34    | Economy class  |
  | 35  | 35    | Economy class  |
  | 36  | 36    | Economy class  |
  | 37  | 37    | Economy class  |
  | 38  | 38    | Economy class  |
  | 39  | 39    | Comfort class  |
  | 40  | 40    | Economy class  |
  | 41  | 41    | Economy class  |
  | 42  | 42    | Economy class  |
  | 43  | 43    | Economy class  |
  | 44  | 44    | Economy class  |
  | 45  | 45    | Comfort class  |
  | 46  | 46    | Comfort class  |
  | 47  | 47    | Economy class  |
  | 48  | 48    | Economy class  |
  | 49  | 49    | Economy class  |
  | 50  | 50    | Economy class  |

## IFNULL and NULLIF functions

In addition to the `IF` function, SQL also has simpler, but less universal functions `IFNULL` and `NULLIF`,
aimed at processing `NULL` values.

### IFNULL syntax

```sql
IFNULL(value, alternative_value);
```

The 'IFNULL`function returns the`value`passed by the first argument if it is not equal to`NULL', otherwise it returns
an `alternative_value'.

### Examples with the IFNULL function

- If the first argument is not `NULL`, then it will be returned.

  ```sql
  SELECT IFNULL("SQL Academy", 'Alternative SQL Academy") AS sql_trainer;
  ```

  | sql_trainer |
  | ----------- |
  | SQL Academy |

- If the first argument is `NULL`, the value passed by the second argument will be returned.

  ```sql
  SELECT IFNULL(NULL, 'Alternative SQL Academy") AS sql_trainer;
  ```

  | sql_trainer             |
  | ----------------------- |
  | Alternative SQL Academy |

### NULLIF syntax

```sql
NULLIF(value_1, value_2);
```

The `NULLIF` function returns `NULL` if `value_1` is equal to `value_2', otherwise it returns `value_1'.

### Examples with the NULLIF function

- If the value of the first argument is equal to the value of the second argument, `NULL` is returned.

  ```sql
  SELECT NULLIF("SQL Academy", "SQL Academy") AS sql_trainer;
  ```

  | sql_trainer |
  | ----------- |
  | null        |

- If the values of the first and second arguments are different, the value of the first argument is returned.

  ```sql
  SELECT NULLIF("SQL Academy", 'Alternative SQL Academy") AS sql_trainer;
  ```

  | sql_trainer |
  | ----------- |
  | SQL Academy |
