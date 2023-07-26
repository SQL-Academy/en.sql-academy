# IS NULL, BETWEEN, IN Operators

We have already familiarized ourselves with the syntax of the `WHERE` operator and comparison operators,
but in addition to them, we can use the following useful operators in conditional queries:

- `IS NULL`
- `BETWEEN`
- `IN`

Let's look at their application.

## IS NULL

The `IS NULL` operator allows you to find out if the checked value is equal to `NULL`, i.e. if the value is empty.

For example, let's select all teachers who do not have a middle name:

```sql
SELECT * FROM Teacher
WHERE middle_name IS NULL;
```

| id  | first_name | middle_name | last_name |
| --- | ---------- | ----------- | --------- |
| 10  | YUrij      | null        | Krylov    |
| 11  | Andrej     | null        | Evseev    |

To use negation, i.e. if we want to find all records where the field is not equal to `NULL`, we must use the following syntax:

```sql
SELECT * FROM Teacher
WHERE middle_name IS NOT NULL;
```

## BETWEEN

The `BETWEEN min AND max` operator allows you to find out if the checked value of the column is located in the interval between `min` and `max`,
including the values `min` and `max` themselves.

It is identical to the condition:

```sql
... WHERE field >= min AND field <= max
```

This operator is used as follows:

```sql
SELECT * FROM Payments
WHERE unit_price BETWEEN 100 AND 500;
```

All records from the `Payments` table where the value of the `unit_price` field is from 100 to 500 will be returned as a result.

## IN

The `IN` operator allows you to find out if the checked value of the column is included in a list of certain values.

```sql
SELECT * FROM FamilyMembers
WHERE status IN ('father', 'mother');
```

| member_id | status | member_name       | birthday                 |
| --------- | ------ | ----------------- | ------------------------ |
| 1         | father | Headley Quincey   | 1960-05-13T00:00:00.000Z |
| 2         | mother | Flavia Quincey    | 1963-02-16T00:00:00.000Z |
| 6         | father | Ernest Forrest    | 1961-09-11T00:00:00.000Z |
| 7         | mother | Constance Forrest | 1968-09-06T00:00:00.000Z |
