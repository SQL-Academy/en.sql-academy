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

## Field enumeration and value correspondence

When using the `INSERT` operator, you can explicitly specify which fields of the table the data will be inserted into. This is done by listing the field names in parentheses after the table name:

```sql
INSERT INTO table_name (field1, field2, field3)
VALUES (value1, value2, value3);
```

**Important rules:**

- The order of values in `VALUES` must strictly correspond to the order of fields in the enumeration
- The number of values must match the number of specified fields
- If a field is not specified in the enumeration, it will receive the default value (if one is set) or `NULL` (if the field allows `NULL`)

For example, if the `Goods` table has fields `good_id`, `good_name`, and `type`, then the following queries are equivalent:

```sql
-- Explicit field specification
INSERT INTO Goods (good_id, good_name, type)
VALUES (20, 'Table', 2);

-- Changed field order - values change accordingly
INSERT INTO Goods (good_name, type, good_id)
VALUES ('Table', 2, 20);
```

If you don't specify the list of fields, then values must be listed for **all** table fields in the order they were defined when creating the table:

```sql
INSERT INTO Goods
VALUES (20, 'Table', 2);
```

> It is recommended to always explicitly specify the list of fields.
> This makes the code more readable, protects against errors when changing the table structure, and allows inserting values only into the required fields.

## Differences between INSERT syntaxes

The `INSERT` operator supports two main syntaxes for specifying data:

### INSERT INTO ... VALUES

Used to insert **predefined** values. Can insert one or more rows at once:

```sql
-- Single row
INSERT INTO Goods (good_id, good_name, type)
VALUES (20, 'Table', 2);

-- Multiple rows
INSERT INTO Goods (good_id, good_name, type)
VALUES
    (20, 'Table', 2),
    (21, 'Chair', 2),
    (22, 'Lamp', 8);
```

**When to use:** for inserting specific, static data that is known in advance.

### INSERT INTO ... SELECT

Used to insert data **obtained from a query**. Allows copying data from one table to another or inserting results of complex calculations:

```sql
INSERT INTO Goods (good_id, good_name, type)
SELECT 20, 'Table', 2;

-- Or copying from another table
INSERT INTO Goods (good_id, good_name, type)
SELECT good_id + 100, good_name, type
FROM Goods
WHERE type = 2;
```

**When to use:** for copying data between tables, inserting calculation results, or when data depends on existing records in the database.

Thus, You can add new entries in the following ways:

Family database ER diagram: [open on SQL Academy](https://sql-academy.org/en/guide/operator-insert).

- Using the syntax `INSERT INTO ... SELECT`

    ```sql
    INSERT INTO Goods (good_id, good_name, type)
    SELECT 20, 'Table', 2;
    ```

- Using the syntax `INSERT INTO ... VALUES (...)`

    ```sql
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
INSERT INTO Goods SELECT MAX(good_id) + 1, 'Table', 2 FROM Goods;
```

Here, we use the `MAX` function to find the maximum value in the primary key column. However, this method is not the most reliable or universal way to determine the primary key value, as it only works best with numeric data types. For all other data types, implementing this approach would be more complex. Additionally, using this method may result in retrieving a value that was previously present in the table but has since been deleted, leading to potential data inconsistencies in the system 💥. Therefore, it is recommended that an alternative method be used in real-world projects.

## Automatic primary key generation

**MySQL**

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

**PostgreSQL**

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
