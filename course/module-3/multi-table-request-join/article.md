# Multi-table queries, JOIN operator

## Multi-table queries

In the previous articles, we described working with only one table of a database.
In reality, however, it is very often necessary to make a selection from several tables, somehow combining them.
In this article, you will learn the main ways to join tables.

For example, if we want to find out about expenses on purchases, we can do so as follows:

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

The `family_member` field in the resulting selection displays the record identifiers from the `Payments` table, but they mean little to us.

Instead of these identifiers, it would be much more informative to output the names of those who made the purchases (the `member_name` field from the `FamilyMember` table).
This is exactly why table joining and the JOIN operator exist.

## The general structure of a multi-table query

```sql
SELECT table_fields
FROM table_1
[INNER] | [[LEFT | RIGHT | FULL][OUTER]] JOIN table_2
    ON join_condition
[[INNER] | [[LEFT | RIGHT | FULL][OUTER]] JOIN table_n
    ON join_condition]
```

As can be seen from the structure, joining can be:

-   internal `INNER` (by default)
-   outer `OUTER`, in which case the outer connection is divided into `LEFT`, `RIGHT`, and `FULL`.

We will learn in more detail in the next articles what the difference is between internal and external joining and how they work.

For now, it is enough for us to know that for the above example with a query for purchases,
we will need an internal connection query that will look like this:

```sql
SELECT family_member, member_name, amount * unit_price AS price FROM Payments
INNER JOIN FamilyMembers
    ON Payments.family_member = FamilyMembers.member_id
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

## Output of all columns from a table in a multi-table query

    Previously, when all queries were executed on one table, it was enough to specify the `*` character to output all fields from this table. Now, when there can be several tables, `*` will mean "output all columns from the tables listed in the expression `FROM`".

In some cases, we may need to output columns belonging only to a particular table. For example, the connection of the `Payments` and `FamilyMembers` tables is given, and only the fields from the `FamilyMembers` table should be output to the final selection. How to do it? It's very simple! It is necessary to add the name of the table before the `*` symbol:

```sql
SELECT FamilyMembers.* FROM Payments
INNER JOIN FamilyMembers
    ON Payments.family_member = FamilyMembers.member_id
```

> Remember this option, you will need it in future tasks 😉

In the same way, you can output **all columns from multiple tables**:

```sql
SELECT Payments.*, FamilyMembers.* FROM Payments
INNER JOIN FamilyMembers
    ON Payments.family_member = FamilyMembers.member_id
```

> In this case, instead of `Payments.*, Family Members.*` you can use `*`, because only these two tables are listed in the `FROM` clause. The output will be the same in both cases.

But what if you need to output **several columns from one table and all from another**? This is also possible! Output the `payment_id` and `family_member` fields from `Payments`, as well as the fields from `FamilyMembers`:

```sql
SELECT payment_id, family_member, FamilyMembers.* FROM Payments
INNER JOIN FamilyMembers
    ON Payments.family_member = FamilyMembers.member_id
```
