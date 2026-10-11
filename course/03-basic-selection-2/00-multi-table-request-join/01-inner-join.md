---
meta:
    title: "INNER JOIN"
    description: "Description and syntax of INNER JOIN"
---

# INNER JOIN

In the previous lesson, we looked at the general structure of a multi-table query:

```sql
SELECT table_fields
FROM table_1
[INNER | {LEFT | RIGHT | FULL} [OUTER]] JOIN table_2
    ON join_condition
[...]
```

- `table_fields` — the columns from any of the joined tables that go into the result.
- `table_1` — the first table that the other tables are joined to.
- `INNER` (keyword, optional) — inner join: only rows that found a match get into the result. This is the default join
  type, so you can omit `INNER`.
- `LEFT | RIGHT | FULL` (keyword, optional) — outer join: left, right or full. Besides the matched pairs, the result also gets the
  unmatched rows of the left, the right or both tables.
- `OUTER` (keyword, optional) — only emphasizes that the join is outer. It goes after `LEFT`, `RIGHT` or `FULL` and
  does not change the result.
- `table_2` — the table being joined. To join more tables, add a separate `JOIN ... ON ...` for each
  of them.
- `join_condition` — the rule by which rows of the two tables find each other, for example
  `Payments.family_member = FamilyMembers.member_id`.

When it comes to a multi-table query with an inner join, the general structure looks like this:

```sql
SELECT table_fields
FROM table_1
[INNER] JOIN table_2
    ON join_condition
[...]
```

- `table_fields` — the columns from any of the joined tables that go into the result.
- `table_1` — the first table that the other tables are joined to.
- `INNER` (keyword, optional) — inner join: only rows that found a match get into the result. This is the default join
  type, so you can omit `INNER`.
- `table_2` — the table being joined. To join more tables, add a separate `JOIN ... ON ...` for each
  of them.
- `join_condition` — the rule by which rows of the two tables find each other, for example
  `Payments.family_member = FamilyMembers.member_id`.

For example, a query might look like this:

Family database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/inner-join).

```sql
SELECT family_member, member_name FROM Payments
INNER JOIN FamilyMembers
    ON Payments.family_member = FamilyMembers.member_id
```

| family_member | member_name     |
| ------------- | --------------- |
| 1             | Headley Quincey |
| 2             | Flavia Quincey  |
| 3             | Andie Quincey   |
| 4             | Lela Quincey    |
| 4             | Lela Quincey    |
| 5             | Annie Quincey   |
| 2             | Flavia Quincey  |
| 2             | Flavia Quincey  |
| 5             | Annie Quincey   |
| 3             | Andie Quincey   |
| 2             | Flavia Quincey  |
| 1             | Headley Quincey |
| 3             | Andie Quincey   |
| 3             | Andie Quincey   |

## Concept of inner join

An inner join is a type of join that returns only the rows that have matching values in both tables based on the join condition.
It creates a new table that includes fields from both the first and second tables.

Visually, this looks like:

![Concept of Inner Join](https://sql-academy.org/static/guidePage/inner-join/inner-join-example.png "Concept of Inner Join")

Since our condition specifies that `Payments.good_id` is equal to `Goods.good_id`,
only the records where both tables have the same `good_id` value will be included in the resulting query.

## Using WHERE to join tables

You can also use the `WHERE` operator to join tables internally. For example, the above query, written using `INNER JOIN`, will look like this:

```sql
SELECT family_member, member_name FROM Payments, FamilyMembers
    WHERE Payments.family_member = FamilyMembers.member_id
```

| family_member | member_name     |
| ------------- | --------------- |
| 1             | Headley Quincey |
| 2             | Flavia Quincey  |
| 3             | Andie Quincey   |
| 4             | Lela Quincey    |
| 4             | Lela Quincey    |
| 5             | Annie Quincey   |
| 2             | Flavia Quincey  |
| 2             | Flavia Quincey  |
| 5             | Annie Quincey   |
| 3             | Andie Quincey   |
| 2             | Flavia Quincey  |
| 1             | Headley Quincey |
| 3             | Andie Quincey   |
| 3             | Andie Quincey   |
