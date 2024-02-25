---
meta:
    title: 'Adding data, insert statement'
    description: 'Adding a new record in sql. Auto-increment of the primary key when creating a row in the table. SQL insert into statement.'
---

# Adding data, INSERT operator

The operator `INSERT` is used to add new records to the table.

## General query structure with the INSERT operator

```sql
INSERT INTO table_name [(table_field, ...)]
VALUES (value_of_table_field, ...)
| SELECT table_field, ... FROM table_name ...
```

Values can be inserted by enumeration using the word `VALUES` listing them in parentheses separated by commas or using the operator `SELECT`.

Thus, You can add new entries in the following ways:

-   Using the syntax `INSERT INTO ... SELECT`

    ```sql-executable-Family-targetTable:Goods
    INSERT INTO Goods (good_id, good_name, type)
    SELECT 20, 'Table', 2;
    ```

-   Using the syntax `INSERT INTO ... VALUES (...)`

    ```sql-executable-Family-targetTable:Goods
    INSERT INTO Goods (good_id, good_name, type)
    VALUES (20, 'Table', 2);
    ```

Each of these queries will give the same result:

| good_id | good_name          | type |
| ------- | ------------------ | ---- |
| 1       | apartment fee      | 1    |
| 2       | phone fee          | 1    |
| 3       | bread              | 2    |
| 4       | milk               | 2    |
| 5       | red caviar         | 3    |
| 6       | cinema             | 4    |
| 7       | black caviar       | 3    |
| 8       | cough tablets      | 5    |
| 9       | potato             | 2    |
| 10      | pineapples         | 3    |
| 11      | television         | 8    |
| 12      | vacuum cleaner     | 8    |
| 13      | jacket             | 7    |
| 14      | fur coat           | 7    |
| 15      | music school fee   | 6    |
| 16      | english school fee | 6    |
| 20      | Table              | 2    |

## Primary key when adding a new record

It should be remembered that the primary key of the table is a unique value and adding an existing value will result in an error.

When adding a new record with unique indices, the choice of such a unique values can be a daunting task. The solution may be an additional request, aimed at identifying the maximum value of the primary key to generate a new unique value.

```sql
INSERT INTO Goods SELECT COUNT(*) + 1, 'Table', 2 FROM Goods;
```

## MySQL

MySQL introduced a mechanism for its automatic generation. To do this, just provide the primary key `good_id` attribute `AUTO_INCREMENT`.
Then when creating a new record as the value `good_id` just pass `NULL` or `0` - the field will automatically get a value greater than the previous one by one.

```sql
CREATE TABLE Goods (
	good_id INT NOT NULL AUTO_INCREMENT
	...
);
```

```sql
INSERT INTO Goods VALUES (NULL, 'Table', 2);
```

## PostgreSQL

PostgreSQL has a similar mechanism for automatically generating a unique identifier.
For this, it has types `SMALLSERIAL`, `SERIAL`, `BIGSERIAL`, which are not real types, but rather just the convenience of writing columns with a unique identifier.
A column with one of the above types will be integer and will automatically grow when a new record is added.

```sql
CREATE TABLE Goods (
	good_id SERIAL
	...
);
```

```sql
INSERT INTO Goods (good_name, type) VALUES ('Table', 2);
```
