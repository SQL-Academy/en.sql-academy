---
meta:
    title: "Conditional logic in SQL"
    description: "Conditional logic in SQL: IF function in MySQL and additional functions in PostgreSQL (COALESCE, NULLIF)"
---

**MySQL**

# Conditional IF function

In the previous lesson, we looked at the `CASE` statement for implementing conditional logic in SQL.
However, this is not the only mechanism by which it is possible to implement logic branching in a query.
It's time to turn our attention to the `IF` function.

## IF Syntax

```sql
IF(conditional_expression, value_1, value_2);
```

If the conditional expression passed as the first argument to the `IF` function is true,
the function will return the value of the second argument `value_1`, otherwise the value of the third argument `value_2` is returned.

**PostgreSQL**

# Additional conditional logic functions

In the previous lesson, we studied the `CASE` statement for implementing conditional logic in SQL.
PostgreSQL provides additional functions that simplify working with conditional logic in special cases.
These functions are especially useful when working with `NULL` values and creating more readable code.

## Functions for conditional logic

In addition to the universal `CASE` operator, PostgreSQL provides:

1. **COALESCE function** - for working with `NULL` values
2. **NULLIF function** - for special cases with `NULL`

These functions are standard SQL functions and make code more readable in certain situations.

### Examples

**MySQL**

- Simple comparison of two numbers. Since 10 is not greater than 20, the function will return 'FALSE'.

    ```sql
    SELECT IF(10>20, "TRUE", "FALSE");
    ```

    | IF(10>20, "TRUE", "FALSE") |
    | -------------------------- |
    | FALSE                      |

**PostgreSQL**

- Simple example of conditional logic using the CASE operator from the previous lesson:

    ```sql
    SELECT CASE WHEN 10 > 20 THEN 'TRUE' ELSE 'FALSE' END;
    ```

    | case  |
    | ----- |
    | FALSE |

**MySQL**

- Example of use with a real database. Based on the price, it is necessary to determine whether the housing belongs to one of two classes: "Comfort class" and "Economy class".
  If the price is greater than or equal to `150`, then this housing belongs to "Comfort class".

    Airbnb database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/if-function).

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
    | 30  | 180   | Comfort class |
    | 31  | 50    | Economy class |
    | 32  | 52    | Economy class |
    | 33  | 55    | Economy class |
    | 34  | 50    | Economy class |
    | 35  | 70    | Economy class |
    | 36  | 89    | Economy class |
    | 37  | 35    | Economy class |
    | 38  | 85    | Economy class |
    | 39  | 150   | Comfort class |
    | 40  | 40    | Economy class |
    | 41  | 68    | Economy class |
    | 42  | 120   | Economy class |
    | 43  | 120   | Economy class |
    | 44  | 135   | Economy class |
    | 45  | 150   | Comfort class |
    | 46  | 150   | Comfort class |
    | 47  | 130   | Economy class |
    | 48  | 110   | Economy class |
    | 49  | 115   | Economy class |
    | 50  | 80    | Economy class |

**PostgreSQL**

- Example with real data. The CASE operator helps categorize housing by price:

    Airbnb database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/if-function).

    ```sql
    SELECT id, price,
        CASE WHEN price >= 150 THEN 'Comfort class' ELSE 'Economy class' END AS category
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
    | 30  | 180   | Comfort class |
    | 31  | 50    | Economy class |
    | 32  | 52    | Economy class |
    | 33  | 55    | Economy class |
    | 34  | 50    | Economy class |
    | 35  | 70    | Economy class |
    | 36  | 89    | Economy class |
    | 37  | 35    | Economy class |
    | 38  | 85    | Economy class |
    | 39  | 150   | Comfort class |
    | 40  | 40    | Economy class |
    | 41  | 68    | Economy class |
    | 42  | 120   | Economy class |
    | 43  | 120   | Economy class |
    | 44  | 135   | Economy class |
    | 45  | 150   | Comfort class |
    | 46  | 150   | Comfort class |
    | 47  | 130   | Economy class |
    | 48  | 110   | Economy class |
    | 49  | 115   | Economy class |
    | 50  | 80    | Economy class |

**MySQL**

- `IF` functions can also be nested within each other, emulating the `CASE` operator.

    ```sql
    SELECT id, price,
        IF(price >= 200, "Business Class",
            IF(price >= 150,
                "Comfort class", "Economy class")) AS category
        FROM Rooms
    ```

    | id  | price | category       |
    | --- | ----- | -------------- |
    | 1   | 149   | Economy class  |
    | 2   | 225   | Business Class |
    | 3   | 150   | Comfort class  |
    | 4   | 89    | Economy class  |
    | 5   | 80    | Economy class  |
    | 6   | 200   | Business Class |
    | 7   | 60    | Economy class  |
    | 8   | 79    | Economy class  |
    | 9   | 79    | Economy class  |
    | 10  | 150   | Comfort class  |
    | 11  | 135   | Economy class  |
    | 12  | 85    | Economy class  |
    | 13  | 89    | Economy class  |
    | 14  | 85    | Economy class  |
    | 15  | 120   | Economy class  |
    | 16  | 140   | Economy class  |
    | 17  | 215   | Business Class |
    | 18  | 140   | Economy class  |
    | 19  | 99    | Economy class  |
    | 20  | 190   | Comfort class  |
    | 21  | 299   | Business Class |
    | 22  | 130   | Economy class  |
    | 23  | 80    | Economy class  |
    | 24  | 110   | Economy class  |
    | 25  | 120   | Economy class  |
    | 26  | 60    | Economy class  |
    | 27  | 80    | Economy class  |
    | 28  | 150   | Comfort class  |
    | 29  | 44    | Economy class  |
    | 30  | 180   | Comfort class  |
    | 31  | 50    | Economy class  |
    | 32  | 52    | Economy class  |
    | 33  | 55    | Economy class  |
    | 34  | 50    | Economy class  |
    | 35  | 70    | Economy class  |
    | 36  | 89    | Economy class  |
    | 37  | 35    | Economy class  |
    | 38  | 85    | Economy class  |
    | 39  | 150   | Comfort class  |
    | 40  | 40    | Economy class  |
    | 41  | 68    | Economy class  |
    | 42  | 120   | Economy class  |
    | 43  | 120   | Economy class  |
    | 44  | 135   | Economy class  |
    | 45  | 150   | Comfort class  |
    | 46  | 150   | Comfort class  |
    | 47  | 130   | Economy class  |
    | 48  | 110   | Economy class  |
    | 49  | 115   | Economy class  |
    | 50  | 80    | Economy class  |

**PostgreSQL**

- CASE is perfect for multiple conditions:

    ```sql
    SELECT id, price,
        CASE
            WHEN price >= 200 THEN 'Business Class'
            WHEN price >= 150 THEN 'Comfort class'
            ELSE 'Economy class'
        END AS category
        FROM Rooms
    ```

    | id  | price | category       |
    | --- | ----- | -------------- |
    | 1   | 149   | Economy class  |
    | 2   | 225   | Business Class |
    | 3   | 150   | Comfort class  |
    | 4   | 89    | Economy class  |
    | 5   | 80    | Economy class  |
    | 6   | 200   | Business Class |
    | 7   | 60    | Economy class  |
    | 8   | 79    | Economy class  |
    | 9   | 79    | Economy class  |
    | 10  | 150   | Comfort class  |
    | 11  | 135   | Economy class  |
    | 12  | 85    | Economy class  |
    | 13  | 89    | Economy class  |
    | 14  | 85    | Economy class  |
    | 15  | 120   | Economy class  |
    | 16  | 140   | Economy class  |
    | 17  | 215   | Business Class |
    | 18  | 140   | Economy class  |
    | 19  | 99    | Economy class  |
    | 20  | 190   | Comfort class  |
    | 21  | 299   | Business Class |
    | 22  | 130   | Economy class  |
    | 23  | 80    | Economy class  |
    | 24  | 110   | Economy class  |
    | 25  | 120   | Economy class  |
    | 26  | 60    | Economy class  |
    | 27  | 80    | Economy class  |
    | 28  | 150   | Comfort class  |
    | 29  | 44    | Economy class  |
    | 30  | 180   | Comfort class  |
    | 31  | 50    | Economy class  |
    | 32  | 52    | Economy class  |
    | 33  | 55    | Economy class  |
    | 34  | 50    | Economy class  |
    | 35  | 70    | Economy class  |
    | 36  | 89    | Economy class  |
    | 37  | 35    | Economy class  |
    | 38  | 85    | Economy class  |
    | 39  | 150   | Comfort class  |
    | 40  | 40    | Economy class  |
    | 41  | 68    | Economy class  |
    | 42  | 120   | Economy class  |
    | 43  | 120   | Economy class  |
    | 44  | 135   | Economy class  |
    | 45  | 150   | Comfort class  |
    | 46  | 150   | Comfort class  |
    | 47  | 130   | Economy class  |
    | 48  | 110   | Economy class  |
    | 49  | 115   | Economy class  |
    | 50  | 80    | Economy class  |

However, for special cases, PostgreSQL provides more specialized functions.

**MySQL**

## IFNULL and NULLIF functions

In addition to the `IF` function, MySQL also has simpler, but less universal functions `IFNULL` and `NULLIF`,
aimed at processing `NULL` values.

### IFNULL syntax

```sql
IFNULL(value, alternative_value);
```

The `IFNULL` function returns the `value` passed by the first argument if it is not equal to `NULL`, otherwise it returns
the `alternative_value`.

**PostgreSQL**

## COALESCE function

The `COALESCE` function is an elegant solution for working with `NULL` values.
It returns the first non-NULL value from the list of arguments.

### Syntax

```sql
COALESCE(value1, value2, ..., valueN);
```

This is much more convenient than writing long CASE expressions to handle NULL.

### Comparison of approaches

Using CASE:

```sql
CASE
    WHEN value1 IS NOT NULL THEN value1
    WHEN value2 IS NOT NULL THEN value2
    ELSE value3
END
```

Using COALESCE (much simpler):

```sql
COALESCE(value1, value2, value3)
```

**MySQL**

### Examples with the IFNULL function

**PostgreSQL**

### Examples with the COALESCE function

**MySQL**

- If the first argument is not equal to `NULL`, then it will be returned.

    ```sql
    SELECT IFNULL("SQL Academy", "Alternative SQL Academy") AS sql_trainer;
    ```

    | sql_trainer |
    | ----------- |
    | SQL Academy |

**PostgreSQL**

- If the first argument is not equal to `NULL`, then it will be returned.

    ```sql
    SELECT COALESCE('SQL Academy', 'Alternative SQL Academy') AS sql_trainer;
    ```

    | coalesce    |
    | ----------- |
    | SQL Academy |

**MySQL**

- If the first argument is equal to `NULL`, then the value passed by the second argument will be returned.

    ```sql
    SELECT IFNULL(NULL, "Alternative SQL Academy") AS sql_trainer;
    ```

    | sql_trainer             |
    | ----------------------- |
    | Alternative SQL Academy |

**PostgreSQL**

- If the first argument is equal to `NULL`, then the next non-NULL value will be returned.

    ```sql
    SELECT COALESCE(NULL, 'Alternative SQL Academy') AS sql_trainer;
    ```

    | coalesce                |
    | ----------------------- |
    | Alternative SQL Academy |

- `COALESCE` can accept multiple arguments, making the code very readable:

    ```sql
    SELECT COALESCE(NULL, NULL, 'SQL Academy', 'Backup option') AS sql_trainer;
    ```

    | coalesce    |
    | ----------- |
    | SQL Academy |

## NULLIF function

The `NULLIF` function is useful when you need to replace a specific value with NULL.
This can be helpful for filtering or processing "empty" values.

### NULLIF syntax

```sql
NULLIF(value_1, value_2);
```

The `NULLIF` function returns `NULL` if `value_1` is equal to `value_2`, otherwise it returns `value_1`.

### Examples with the NULLIF function

**MySQL**

- If the value of the first argument is equal to the value of the second argument, then `NULL` is returned.

    ```sql
    SELECT NULLIF("SQL Academy", "SQL Academy") AS sql_trainer;
    ```

    | sql_trainer |
    | ----------- |
    | null        |

**PostgreSQL**

- If the value of the first argument is equal to the value of the second argument, then `NULL` is returned.

    ```sql
    SELECT NULLIF('SQL Academy', 'SQL Academy') AS sql_trainer;
    ```

    | nullif |
    | ------ |
    | null   |

**MySQL**

- If the values of the first and second arguments are different, then the value of the first argument is returned.

    ```sql
    SELECT NULLIF("SQL Academy", "Alternative SQL Academy") AS sql_trainer;
    ```

    | sql_trainer |
    | ----------- |
    | SQL Academy |

**PostgreSQL**

- If the values of the first and second arguments are different, then the value of the first argument is returned.

    ```sql
    SELECT NULLIF('SQL Academy', 'Alternative SQL Academy') AS sql_trainer;
    ```

    | nullif      |
    | ----------- |
    | SQL Academy |

### When to use each function:

- **CASE**: When you need complex conditional logic with multiple conditions
- **COALESCE**: When you need to replace NULL values with default values
- **NULLIF**: When you need to turn specific values into NULL

These functions make code more readable and are part of the SQL standard.
