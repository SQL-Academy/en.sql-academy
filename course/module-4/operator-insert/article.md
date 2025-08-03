---
meta:
  title: "Adding data, insert statement"
  description: "Adding a new record in sql. Auto-increment of the primary key when creating a row in the table. SQL insert into statement."
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

- Using the syntax `INSERT INTO ... SELECT`

  ```sql-executable-Family-targetTable:Goods
  INSERT INTO Goods (good_id, good_name, type)
  SELECT 20, 'Table', 2;
  ```

- Using the syntax `INSERT INTO ... VALUES (...)`

  ```sql-executable-Family-targetTable:Goods
  INSERT INTO Goods (good_id, good_name, type)
  VALUES (20, 'Table', 2);
  ```

Each of these queries will give the same result:

## Primary key when adding a new record

It should be remembered that the primary key of the table is a unique value and adding an existing value will result in an error.

When adding a new record with unique indices, the choice of such a unique values can be a daunting task. The solution may be an additional request, aimed at identifying the maximum value of the primary key to generate a new unique value.

```sql-executable-Family-targetTable:Goods
INSERT INTO Goods SELECT MAX(good_id) + 1, 'Table', 2 FROM Goods;
```

Here, we use the `MAX` function to find the maximum value in the primary key column. However, this method is not the most reliable or universal way to determine the primary key value, as it only works best with numeric data types. For all other data types, implementing this approach would be more complex. Additionally, using this method may result in retrieving a value that was previously present in the table but has since been deleted, leading to potential data inconsistencies in the system 💥. Therefore, it is recommended that an alternative method be used in real-world projects.

## Automatic primary key generation

<MySQLOnly>

MySQL introduced a mechanism for automatic primary key generation. To do this, just provide the primary key `good_id` attribute `AUTO_INCREMENT`.
Then when creating a new record as the value `good_id` just pass `NULL` or `0` - the field will automatically get a value greater than the previous one by one.

```sql
CREATE TABLE Goods (
	good_id INT NOT NULL AUTO_INCREMENT,
	good_name VARCHAR(255),
	type INT
);
```

```sql
INSERT INTO Goods VALUES (NULL, 'Table', 2);
```

</MySQLOnly>

<PostgreSQLOnly>

PostgreSQL has a mechanism for automatically generating a unique identifier.
For this, it has types `SMALLSERIAL`, `SERIAL`, `BIGSERIAL`, which are not real types, but rather just the convenience of writing columns with a unique identifier.
A column with one of the above types will be integer and will automatically grow when a new record is added.

```sql
CREATE TABLE Goods (
	good_id SERIAL,
	good_name VARCHAR(255),
	type INT
);
```

```sql
INSERT INTO Goods (good_name, type) VALUES ('Table', 2);
```

</PostgreSQLOnly>
