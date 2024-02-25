# Combining requests, Union operator

The results of executing SQL queries can be combined. There is a `UNION` operator for this.

## The general structure of a query with the operator UNION

```sql
SELECT table_fields FROM list_of_tables ...
UNION [ALL]
SELECT table_fields FROM list_of_tables ... ;
```

`UNION` removes repetitions in the resulting table by default. There is an optional `ALL` parameter for repeat display.

-   Do not confuse query join operations with table join operations. To do this, use the `JOIN` operator.
-   Do not confuse query merge operations with subqueries. Subqueries are executed for linked tables.

Table joining with the `UNION` operator is performed for tables that are not related in any way, but with a similar structure.

In order for `UNION` to function correctly, it is essential that the resulting tables from each of the SQL queries contain the same number of columns with the same data types and in the same order.

There are two other operators whose behavior is very similar to `UNION`:

-   `INTERSECT`
    Combines two SELECT queries, but returns only the first SELECT records that have matches in the second SELECT element.
-   `EXCEPT`
    Combines two SELECT queries, but returns only the first SELECT records, which do not match in the second SELECT element.

## Examples of using

For example, it is necessary to display the name of all goods and the names of all family members (very conditional task). Since the data types are the same, we can do this.

```sql
SELECT DISTINCT Goods.good_name AS name FROM Goods
UNION
SELECT DISTINCT FamilyMembers.member_name AS name FROM FamilyMembers;
```

| name               |
| ------------------ |
| apartment fee      |
| phone fee          |
| bread              |
| milk               |
| red caviar         |
| cinema             |
| black caviar       |
| cough tablets      |
| potato             |
| pineapples         |
| television         |
| vacuum cleaner     |
| jacket             |
| fur coat           |
| music school fee   |
| english school fee |
| Headley Quincey    |
| Flavia Quincey     |
| Andie Quincey      |
| Lela Quincey       |
| Annie Quincey      |
| Ernest Forrest     |
| Constance Forrest  |
| Wednesday Addams   |
