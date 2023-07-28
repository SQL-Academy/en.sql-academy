# Correlated subqueries

All previous subqueries we've discussed were uncorrelated (independent).
They could be executed autonomously from the main query, and we could see what they returned before their result was used in the main query.
Correlated subqueries, on the other hand, refer to one or more columns in the main query.

## Example of a correlated subquery

For example, the following correlated subquery finds out who spent how much:

```sql
SELECT FamilyMembers.member_name, (
    SELECT SUM(Payments.unit_price * Payments.amount)
    FROM Payments
    WHERE Payments.family_member = FamilyMembers.member_id
) AS total_spent
FROM FamilyMembers;
```

| member_name       | total_spent |
| ----------------- | ----------- |
| Headley Quincey   | 2504        |
| Flavia Quincey    | 74194       |
| Andie Quincey     | 3600        |
| Lela Quincey      | 650         |
| Annie Quincey     | 1060        |
| Ernest Forrest    | <NULL>      |
| Constance Forrest | <NULL>      |
| Wednesday Addams  | <NULL>      |

In this case, the correlated subquery refers to the `member_id` column in the main query.

A correlated subquery differs from an uncorrelated subquery in that it is not executed once before the query in which it is nested, but for each row that can be included in the final result.

In our case, the main query to the `FamilyMembers` table returns 8 records, and a correlated subquery is executed for each of them.

## Performance impact

It should be noted that the use of correlated subqueries can cause performance problems, especially if the containing query returns a lot of rows,
since the correlated subquery will be executed separately for each row of the containing query.
