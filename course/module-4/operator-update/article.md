# Syntax INSERT operator

To edit existing records in tables, there is an SQL operator `UPDATE`.

## General query structure with the UPDATE operator

```sql
UPDATE table_name
SET table_field1 = table_field_value1,
    table_fieldN = table_field_valueN
[WHERE the_conditions_of_the_limitations]
```

So, for example, if you need to change the name, the request will look like this:

```sql
UPDATE FamilyMembers
SET member_name = "Andie Anthony"
WHERE member_name = "Andie Quincey";
```

| member_id | status      | member_name       | birthday             |
| --------- | ----------- | ----------------- | -------------------- |
| 1         | father      | Headley Quincey   | 1960-05-13T00:00:00Z |
| 2         | mother      | Flavia Quincey    | 1963-02-16T00:00:00Z |
| 3         | varchar(50) | Andie Anthony     | 1983-06-05T00:00:00Z |
| 4         | daughter    | Lela Quincey      | 1985-06-07T00:00:00Z |
| 5         | daughter    | Annie Quincey     | 1988-04-10T00:00:00Z |
| 6         | father      | Ernest Forrest    | 1961-09-11T00:00:00Z |
| 7         | mother      | Constance Forrest | 1968-09-06T00:00:00Z |
| 8         | daughter    | Wednesday Addams  | 2005-01-13T00:00:00Z |

> Be careful when updating data. If you skip the `WHERE` operator, all entries will be updated.

## Calculated Values

In data update requests, you can change values based on previous values.

```sql
UPDATE Payments
SET unit_price = unit_price * 2;
```

It is also allowed to assign values of some columns to other columns. But at the same time, of course, column types must be compatible.
