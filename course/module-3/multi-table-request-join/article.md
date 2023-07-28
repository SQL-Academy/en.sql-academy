# Multi-table queries, JOIN operator

## Multi-table queries

In the previous articles, we described working with only one table of a database.
In reality, however, it is very often necessary to make a selection from several tables, somehow combining them.
In this article, you will learn the main ways to join tables.

For example, if we want to get information about expenses on purchases, we can get it as follows:

```sql
SELECT family_member, amount * unit_price AS price FROM Payments
```

| family_member | price |
| ------------- | ----- |
| 1             | 2000  |
| 2             | 2100  |
| 3             | 100   |
| 4             | 350   |
| 4             | 300   |
| 5             | 100   |
| 2             | 120   |
| 2             | 5500  |
| 5             | 230   |
| 3             | 2200  |
| 2             | 66000 |
| 1             | 40    |
| 3             | 100   |
| 3             | 1200  |

The `family_member` field in the resulting selection displays the record identifiers from the `FamilyMembers` table, but they mean little to us.

Instead of these identifiers, it would be much more informative to output the names of those who made the purchases (the `member_name` field from the `FamilyMember` table).
This is exactly why table joining and the JOIN operator exist.

## The general structure of a multi-table query

```sql
SELECT table_fields
FROM table_1
[INNER] | [[LEFT | RIGHT | FULL][OUTER]] JOIN table_2
    ON join_condition
[INNER] | [[LEFT | RIGHT | FULL][OUTER]] JOIN table_n
    ON join_condition]
```

As can be seen from the structure, joining can be:

- internal `INNER` (by default)
- outer `OUTER`, in which case the outer connection is divided into `LEFT`, `RIGHT`, and `FULL`.

We will learn in more detail in the next articles what the difference is between internal and external joining and how they work.

For now, it is enough for us to know that for the above example with a query for purchases,
we will need an internal connection query that will look like this:

```sql
SELECT family_member, member_name, amount * unit_price AS price FROM Payments
INNER JOIN FamilyMembers ON Payments.family_member = FamilyMembers.member_id
```

| family_member | member_name     | price |
| ------------- | --------------- | ----- |
| 1             | Headley Quincey | 2000  |
| 2             | Flavia Quincey  | 2100  |
| 3             | Andie Quincey   | 100   |
| 4             | Lela Quincey    | 350   |
| 4             | Lela Quincey    | 300   |
| 5             | Annie Quincey   | 100   |
| 2             | Flavia Quincey  | 120   |
| 2             | Flavia Quincey  | 5500  |
| 5             | Annie Quincey   | 230   |
| 3             | Andie Quincey   | 2200  |
| 2             | Flavia Quincey  | 66000 |
| 1             | Headley Quincey | 40    |
| 3             | Andie Quincey   | 100   |
| 3             | Andie Quincey   | 1200  |

In this query, we match records from the `Payments` table with records from the `FamilyMembers` table.

To make the matching work, we specify how exactly records from two different tables should find each other. This condition is specified after `ON`:

```sql
ON Payments.family_member = FamilyMembers.member_id
```

In our case, the `family_member` field points to the identifier in the `FamilyMembers` table and thus helps to unambiguously match.

> In most cases, the connection condition is the equality of columns in tables `(table_1.field = table_2.field)`, but other comparison operators can also be used.
