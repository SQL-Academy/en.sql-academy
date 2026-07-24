---
meta:
    title: "Structure of a Relational Database: Tables, Records and Keys"
    description: "How a relational database is organized inside: tables, records and attributes, primary and foreign keys, column data types — explained with examples and exercises."
---

# Structure of Relational Databases

We have briefly introduced ourselves to relational databases in the <a href="https://sql-academy.org/en/guide/relation-databases" target="_blank">previous article</a>. But
a superficial understanding is not enough for us, is it?
Let's go beyond the surface and delve deeper into the structure and terminology of relational databases.

## Table Structure

In relational databases, information is stored in tables linked to each other. The tables themselves consist of:

- rows, which are called "records"
- columns, which are called "fields" or "attributes"

![Table Structure](https://sql-academy.org/static/guidePage/structure-of-relation-databases/en_structure_db.png "Table Structure")

In each table, each column has a predetermined data type. For example, these types can be:

- `VARCHAR` (string data type)
- `INTEGER` (numeric data type)
- `DATETIME` (date and time data type)
- and others

And each row in the table must have the corresponding type for each column. The DBMS will not allow an attempt to add an arbitrary string to a field with the `DATETIME` type.

To find out attribute data types, you can execute an SQL command and specify the table name:

**MySQL**

```sql
DESCRIBE FamilyMembers
```

| Field       | Type        | Null | Key | Default | Extra |
| ----------- | ----------- | ---- | --- | ------- | ----- |
| member_id   | int         | NO   | PRI |         |       |
| status      | varchar(50) | NO   |     |         |       |
| member_name | varchar(50) | NO   |     |         |       |
| birthday    | datetime    | NO   |     |         |       |

**PostgreSQL**

```sql
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'familymembers'
  AND table_schema = current_schema();
```

| column_name | data_type                   | is_nullable |
| ----------- | --------------------------- | ----------- |
| member_id   | integer                     | NO          |
| status      | character varying           | NO          |
| member_name | character varying           | NO          |
| birthday    | timestamp without time zone | NO          |

Alternatively, you can look at the ERD diagram of the database schema:

Family database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/structure-of-relation-databases).

## Primary Key

Any database management system has a built-in system of data integrity and consistency. This system works on a set of rules defined in the database schema.
Primary keys and foreign keys are just some of these rules.

To avoid ambiguity in searching tables, there are primary keys, or "key fields".

> A key field (primary key) is a field (or set of fields) whose value uniquely identifies a record in the table.

If we refer to our aforementioned table, `FamilyMembers`, then its key field is `member_id`.
Using this rule, the DBMS will not allow us to create a new record where the `member_id` field is not unique.

It is worth noting that the presence of a primary key is not necessary, and data integrity can be determined, for example, at the application level.

## Foreign Key

> A foreign key is a field (or set of fields) in one table that refers to the primary key in another table.

The table with the foreign key is called the child table, and the table with the primary key is called the referenced or parent table.

The foreign key rule guarantees that when creating records in the child table, the value of the field that is the foreign key exists in the parent table.

![Examples of foreign keys](https://sql-academy.org/static/guidePage/structure-of-relation-databases/en_keys.png "Examples of foreign keys")

The presence of a foreign key is the same optional requirement as in the case of a primary key.

If the foreign key is not defined, the database management system will still work, but it will not verify that, for example,
when creating a record in the `Purchase` table, the `buyer_id` and `good_id` fields contain
values that are defined in the corresponding tables in the `id` field.

Which of the following statements is **false** regarding keys in relational databases?

1. There can only be one primary key in each table — There can only be one primary key in each table, which uniquely identifies a record in the table. The primary key can consist of multiple fields in the table, but there is only one primary key.

2. A table can contain multiple foreign keys or no foreign keys at all — Defining a foreign key constraint is optional, so a table may not have any foreign keys. At the same time, if a table has multiple fields that reference the identifiers of other tables, we can define multiple foreign keys.

3. **Correct answer: **The purpose of primary key rules and foreign key rules is the same — The purposes of primary key rules and foreign key rules are different. The primary key ensures the uniqueness of each record within a single table, while the foreign key is used to maintain referential integrity.
