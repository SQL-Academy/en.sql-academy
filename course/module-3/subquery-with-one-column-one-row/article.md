---
meta:
    title: 'Subquery with one row and one column'
    description: 'Scalar subqueries in SQL, examples and syntax'
---

# Subquery with one row and one column

In this lesson, let's take a closer look at a subquery that returns one row and one column.
This type of subquery is also known as a scalar subquery.

It can be used in various parts of the main SQL query, but most often it is used
in selection restriction conditions using comparison operators (`=`, `<>`, `>`, `<`).

## Examples

The following simplest query demonstrates the output of a single value (company name).
In this form, it doesn't make much sense, but your queries can be much more complex.

```sql-Trip
SELECT (SELECT name FROM company LIMIT 1) AS company_name;
```

| company_name |
| ------------ |
| Don_avia     |

Similarly, scalar subqueries can be used to filter rows using `WHERE`, using comparison operators.

```sql
SELECT * FROM FamilyMembers
    WHERE birthday = (SELECT MAX(birthday) FROM FamilyMembers);
```

| member_id | status   | member_name      | birthday                 |
| --------- | -------- | ---------------- | ------------------------ |
| 8         | daughter | Wednesday Addams | 2005-01-13T00:00:00.000Z |

With this query, it is possible to obtain the youngest family member.
The subquery in this case is necessary to obtain the maximum date of birth, which is then used in the main query to filter rows.

## The importance of matching the operator used and the subquery result

When using the result of a subquery with comparison operators, as in our example, it is important that the subquery returns exactly a scalar value (1 row and 1 column).

If this subquery returned multiple values, the DBMS would return an error, indicating that it expected the subquery to return only 1 record:
«ER_SUBQUERY_NO_1_ROW: Subquery returns more than 1 row».

Therefore, it is important to be careful when writing subqueries and to understand what result the subquery will return,
and what operators we can use together with the resulting set.
