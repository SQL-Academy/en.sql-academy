---
meta:
    title: "Conditional operator WHERE"
    description: "Conditional operator WHERE in an SQL query. Logical and comparison operators. SELECT FROM WHERE examples"
---

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

**MySQL**

```sql
SELECT * FROM Student
WHERE first_name = "Grigorij" AND YEAR(birthday) > 2000;
```

**PostgreSQL**

```sql
SELECT * FROM Student
WHERE first_name = 'Grigorij' AND EXTRACT(YEAR FROM birthday) > 2000;
```

| id  | first_name | middle_name | last_name | birthday                 | address                         |
| --- | ---------- | ----------- | --------- | ------------------------ | ------------------------------- |
| 33  | Grigorij   | Gennadevich | Kapustin  | 2001-12-13T00:00:00.000Z | ul. Pervomajskaya, d. 45, kv. 6 |
| 65  | Grigorij   | Kirillovich | Kolobov   | 2003-07-17T00:00:00.000Z | ul. CHernova, d. 9, kv. 34      |

This code snippet uses two conditions:

**MySQL**

- `first_name = "Grigorij"` — the student's first name must be "Grigorij"
- `YEAR(birthday) > 2000` — birth year greater than 2000

**PostgreSQL**

- `first_name = 'Grigorij'` — the student's first name must be "Grigorij"
- `EXTRACT(YEAR FROM birthday) > 2000` — birth year greater than 2000

Between them is the logical operator `AND`, which requires both conditions to be met simultaneously. As a result, we get only those students who meet both criteria.

## Comparison operators

To compare values in SQL, special operators are used. They allow you to check whether values are equal, whether one is greater than another, whether they are not equal, and so on. The result of comparison can be:

- `true` (same as 1)
- `false` (same as 0)
- `NULL` (if the comparison result is undefined, for example, when comparing with NULL)

**MySQL**

| Operator              | Sign         | Description                                                                                                                                     |
| :-------------------- | :----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Equality              | `=`          | If both values are equal, then the result will be 1, otherwise 0                                                                                |
| Equivalence           | `<=>`        | Similar to the equality operator, except that the result will be 1 when comparing `NULL` with `NULL` and 0 when comparing any value with `NULL` |
| Inequality            | `<>` or `!=` | If both values are not equal, then the result will be 1, otherwise 0                                                                            |
| Less than             | `<`          | If one value is less than the other, the result will be 1, otherwise 0                                                                          |
| Less than or equal    | `<=`         | If one value is less than or equal to the other, the result will be 1, otherwise 0                                                              |
| Greater than          | `>`          | If one value is greater than the other, the result will be 1, otherwise 0                                                                       |
| Greater than or equal | `>=`         | If one value is greater than or equal to the other, the result will be 1, otherwise 0                                                           |

> The result of comparing any value with `NULL` is `NULL`. The exception is the equivalence operator.

**PostgreSQL**

| Operator              | Sign         | Description                                                                                  |
| :-------------------- | :----------- | -------------------------------------------------------------------------------------------- |
| Equality              | `=`          | If both values are equal, then the result will be true, otherwise false                      |
| Inequality            | `<>` or `!=` | If both values are not equal, then the result will be true, otherwise false                  |
| Less than             | `<`          | If one value is less than the other, the result will be true, otherwise false                |
| Less than or equal    | `<=`         | If one value is less than or equal to the other, the result will be true, otherwise false    |
| Greater than          | `>`          | If one value is greater than the other, the result will be true, otherwise false             |
| Greater than or equal | `>=`         | If one value is greater than or equal to the other, the result will be true, otherwise false |

> The result of comparing any value with `NULL` is `NULL`.

Try playing with these operators in the sandbox yourself and see what results they give:

**MySQL**

```sql
SELECT
    2 = 1,
	'a' = 'a',
    1 <=> NULL,
	NULL <=> NULL,
	2 <> 2,
	3 < 4,
	10 <= 10,
	7 > 1,
	8 >= 10;
```

| 2 = 1 | 'a' = 'a' | 1 \<=> NULL | NULL \<=> NULL | 2 \<> 2 | 3 \< 4 | 10 \<= 10 | 7 > 1 | 8 >= 10 |
| ----- | --------- | ----------- | -------------- | ------- | ------ | --------- | ----- | ------- |
| 0     | 1         | 0           | 1              | 0       | 1      | 1         | 1     | 0       |

**PostgreSQL**

```sql
SELECT
    2 = 1 AS "2 = 1",
	'a' = 'a' AS "'a' = 'a'",
	2 <> 2 AS "2 <> 2",
	3 < 4 AS "3 < 4",
	10 <= 10 AS "10 <= 10",
	7 > 1 AS "7 > 1",
	8 >= 10 AS "8 >= 10";
```

| 2 = 1 | 'a' = 'a' | 2 \<> 2 | 3 \< 4 | 10 \<= 10 | 7 > 1 | 8 >= 10 |
| ----- | --------- | ------- | ------ | --------- | ----- | ------- |
| false | true      | false   | true   | true      | true  | false   |

## Logical operators

Logical operators are your helpers when you need to combine several conditions in one SQL query. With their help, you can flexibly select only those rows that you really need. Let's see how this works in practice:

- `AND` — both conditions must be true.

    Imagine you are looking for flights that simultaneously meet two requirements: for example, the aircraft must be of a certain model, and depart from a specific city. The `AND` operator helps combine these conditions.

    **MySQL**

    ```sql
    SELECT * FROM Trip
    WHERE plane = 'Boeing' AND town_from = 'London';
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM Trip
    WHERE plane = 'Boeing' AND town_from = 'London';
    ```

    | id   | company | plane  | town_from | town_to   | time_out                 | time_in                  |
    | ---- | ------- | ------ | --------- | --------- | ------------------------ | ------------------------ |
    | 7771 | 5       | Boeing | London    | Singapore | 1900-01-01T01:00:00.000Z | 1900-01-01T11:00:00.000Z |
    | 7773 | 5       | Boeing | London    | Singapore | 1900-01-01T03:00:00.000Z | 1900-01-01T13:00:00.000Z |
    | 7775 | 5       | Boeing | London    | Singapore | 1900-01-01T09:00:00.000Z | 1900-01-01T20:00:00.000Z |
    | 7777 | 5       | Boeing | London    | Singapore | 1900-01-01T18:00:00.000Z | 1900-01-02T06:00:00.000Z |
    | 8881 | 5       | Boeing | London    | Paris     | 1900-01-01T03:00:00.000Z | 1900-01-01T04:00:00.000Z |

    This query will select only those flights where the aircraft model is `Boeing` and the departure city is `London`.

    If at least one of the conditions is not met (for example, the aircraft is not `Boeing` or the departure is not from `London`), such a flight will not be included in the result.

- `OR` — it's enough for at least one condition to be met.

    The `OR` operator works as "or". If at least one of the conditions is true — the row will be included in the result. This is convenient when you want to see all flights that meet at least one of your criteria.

    **MySQL**

    ```sql
    SELECT * FROM Trip
    WHERE town_to = 'Paris' OR plane = 'Airbus';
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM Trip
    WHERE town_to = 'Paris' OR plane = 'Airbus';
    ```

    | id   | company | plane  | town_from | town_to | time_out                 | time_in                  |
    | ---- | ------- | ------ | --------- | ------- | ------------------------ | ------------------------ |
    | 1100 | 4       | Boeing | Rostov    | Paris   | 1900-01-01T14:30:00.000Z | 1900-01-01T17:50:00.000Z |
    | 8881 | 5       | Boeing | London    | Paris   | 1900-01-01T03:00:00.000Z | 1900-01-01T04:00:00.000Z |

    As a result, you will get all flights that arrive in `Paris`, as well as all flights on an `Airbus` aircraft (even if they don't fly to `Paris`).

    If a flight is both on an `Airbus` and to `Paris` — it will also be included in the result.

- `NOT` — the condition becomes opposite.

    The `NOT` operator inverts the condition: if it was true, it becomes false, and vice versa. This is convenient when you want to exclude certain values.

    **MySQL**

    ```sql
    SELECT * FROM Trip WHERE NOT town_to = 'Moscow';
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM Trip WHERE NOT town_to = 'Moscow';
    ```

    This query will select all flights that arrive **not** in `Moscow`.

    That is, if the arrival city is `Moscow`, such a flight will not be included in the result. Everything else — will be included.

- `XOR` — this is an operator that helps select rows where only one of two conditions is met, but not both at the same time.

    Suppose you want to find flights that either depart from `Moscow` or arrive in `Paris`, but not both options at the same time. Let's look at all possible cases:

    | Departs <br />from Moscow | Arrives <br />in Paris | Included <br />in result | Explanation                                  |
    | :-----------------------: | :--------------------: | :----------------------: | -------------------------------------------- |
    |            Yes            |           No           |          ✅ Yes          | Only the first condition is met              |
    |            No             |          Yes           |          ✅ Yes          | Only the second condition is met             |
    |            Yes            |          Yes           |          ❌ No           | Both conditions are met — XOR excludes these |
    |            No             |           No           |          ❌ No           | Neither condition is met                     |

    **MySQL**

    ```sql
    SELECT * FROM trip
    WHERE town_from = 'Moscow' XOR town_to = 'Paris';
    ```

    **PostgreSQL**

    ```sql
    SELECT * FROM trip
    WHERE (town_from = 'Moscow' AND town_to != 'Paris')
       OR (town_from != 'Moscow' AND town_to = 'Paris');
    ```

    **MySQL**

    > Note: the XOR operator is not available in all databases. If it's not available, you can use a combination of AND and OR.

    **PostgreSQL**

    > PostgreSQL does not have the XOR operator, so a combination of AND and OR is used to achieve the same result.

## Priority of logical operators

When you write a query with several conditions, SQL must understand in what order to check them. This is similar to mathematics: multiplication first, then addition. Logical operators in SQL also have their own order — this is called priority.

- First — `NOT`
- Then — `AND`
- Then — `XOR`
- Finally — `OR`

**Why is this important?**

It might seem that conditions will be checked simply from left to right, but this is not so! If you don't consider the order, you can get an unexpected result.

Let's look at such an example:

**MySQL**

```sql
SELECT *
FROM Trip
WHERE town_to = 'Paris'
	OR plane = 'Boeing'
	AND NOT town_from = 'Moscow';
```

**PostgreSQL**

```sql
SELECT *
FROM Trip
WHERE town_to = 'Paris'
	OR plane = 'Boeing'
	AND NOT town_from = 'Moscow';
```

| id   | company | plane  | town_from | town_to   | time_out                 | time_in                  |
| ---- | ------- | ------ | --------- | --------- | ------------------------ | ------------------------ |
| 1100 | 4       | Boeing | Rostov    | Paris     | 1900-01-01T14:30:00.000Z | 1900-01-01T17:50:00.000Z |
| 1101 | 4       | Boeing | Paris     | Rostov    | 1900-01-01T08:12:00.000Z | 1900-01-01T11:45:00.000Z |
| 7771 | 5       | Boeing | London    | Singapore | 1900-01-01T01:00:00.000Z | 1900-01-01T11:00:00.000Z |
| 7772 | 5       | Boeing | Singapore | London    | 1900-01-01T12:00:00.000Z | 1900-01-02T02:00:00.000Z |
| 7773 | 5       | Boeing | London    | Singapore | 1900-01-01T03:00:00.000Z | 1900-01-01T13:00:00.000Z |
| 7774 | 5       | Boeing | Singapore | London    | 1900-01-01T14:00:00.000Z | 1900-01-02T06:00:00.000Z |
| 7775 | 5       | Boeing | London    | Singapore | 1900-01-01T09:00:00.000Z | 1900-01-01T20:00:00.000Z |
| 7776 | 5       | Boeing | Singapore | London    | 1900-01-01T18:00:00.000Z | 1900-01-02T08:00:00.000Z |
| 7777 | 5       | Boeing | London    | Singapore | 1900-01-01T18:00:00.000Z | 1900-01-02T06:00:00.000Z |
| 7778 | 5       | Boeing | Singapore | London    | 1900-01-01T22:00:00.000Z | 1900-01-02T12:00:00.000Z |
| 8881 | 5       | Boeing | London    | Paris     | 1900-01-01T03:00:00.000Z | 1900-01-01T04:00:00.000Z |
| 8882 | 5       | Boeing | Paris     | London    | 1900-01-01T22:00:00.000Z | 1900-01-01T23:00:00.000Z |

What happens here:

- First, it checks that the flight is **not** from `Moscow` (`NOT town_from = 'Moscow'`).

- Then it checks that the aircraft is `Boeing`, and combines this with the first condition through `AND`, that is, it looks for flights that are **not** from `Moscow` and on `Boeing` (`plane = 'Boeing' AND NOT town_from = 'Moscow'`)

- Then all flights that arrive in `Paris` are added to the result, even if they are **not** on `Boeing` and depart from `Moscow` (`town_to = 'Paris' OR ...`).

As a result, you get the following selection:

- All flights that are **not** from `Moscow` **and** on `Boeing`
- Plus all flights that arrive in `Paris`

If you want to change the order of condition checking or make it clearer, use **parentheses**. Everything in parentheses is executed first. For example, if you manually place parentheses according to the priorities of logical operators, it immediately becomes clear how the query will be executed!

**MySQL**

```sql
SELECT *
FROM Trip
WHERE (
		town_to = 'Paris'
		OR (
			plane = 'Boeing'
			AND (NOT town_from = 'Moscow')
		)
	);
```

**PostgreSQL**

```sql
SELECT *
FROM Trip
WHERE (
		town_to = 'Paris'
		OR (
			plane = 'Boeing'
			AND (NOT town_from = 'Moscow')
		)
	);
```

Now try it yourself: suppose you want to get a list of flights that depart from `Paris` or on a `Boeing` aircraft, and fly **not** to `Moscow`. This can be done by correctly placing parentheses in the original query from the example. How would you do it?

1. town_from = 'Paris' OR (plane = 'Boeing' AND NOT town_to = 'Moscow'); — This option will select all flights from Paris, as well as Boeing flights that do not fly to Moscow. The parentheses are placed incorrectly: flights from Paris will be included in the selection even if they fly to Moscow.

2. **Correct answer: **(town_from = 'Paris' OR plane = 'Boeing') AND NOT town_to = 'Moscow'; — This query will select flights that either depart from Paris or are on Boeing, and do not fly to Moscow.

3. town_from = 'Paris' OR plane = 'Boeing' AND NOT town_to = 'Moscow'; — This option without parentheses will not work as expected: AND will be executed first, then OR. This will lead to an incorrect result
