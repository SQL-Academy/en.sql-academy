---
meta:
    title: 'Creating and deleting tables'
    description: 'SQL create and delete tables. The describe operator.'
---

# Creating and deleting tables

## Creating a table

Before creating the table, you need to select the database to which the table will be written. This is done using the `USE` statement:

```sql
USE database_name;
```

The `CREATE TABLE` statement is used to create the table. Its simplest use is as follows:

```sql
CREATE TABLE [IF NOT EXIST] table_name (
     column_1 data type,
    [column_2 data type,]
    ...
    [column_n data type,]
);
```

For example, let's create a table of users.

```sql
CREATE TABLE Users (
    id INT,
    name VARCHAR(255),
    age INT
);
```

`INT`, `VARCHAR (255)` - data types: numeric and string, respectively. More details about them can be found in the following articles.

## Additional column definition options

The above definition of columns in a table is simplified. In addition to the name of the column and its type,
it is sometimes necessary to add the following optional parameters to the definition:

- `PRIMARY KEY`

  Specifies a column or set of columns as the primary key.

- `AUTO_INCREMENT`

  Indicates that the value of this column will be automatically increased when new records are added to the table. Each table has a maximum of one `AUTO_INCREMENT` column.
  It is worth noting that this parameter can only be applied to integer and floating point types.

- `UNIQUE`

  Indicates that the values in this column for all records must be different from each other.

- `NOT NULL`

  Indicates that the values in this column must be different from `NULL`.

- `DEFAULT`

  Specifies the default value. This parameter does not apply to the `BLOB`, `TEXT`, `GEOMETRY` and `JSON`.

For our table of users, you can specify the following parameters:

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    age INT NOT NULL DEFAULT 18
);
```

So, in this example:

- `id` - a numeric field that is the primary key;
- `name` - a string type field with a maximum length of 255 characters, which is mandatory;
- `age` - a numeric field with a default value of 18.

## Description of the table

To view the description of the created table, you can use the operator `DESCRIBE`.

```sql
DESCRIBE Users;
```

| Field | Type         | Null | Key | Default | Extra |
| ----- | ------------ | ---- | --- | ------- | ----- |
| id    | int(11)      | NO   | PRI | <NULL>  |       |
| name  | varchar(255) | NO   |     | <NULL>  |       |
| age   | int(11)      | NO   |     | 18      |       |

## Additional table definition options

In addition to the description of the columns, when creating a table, you can additionally specify the following parameters:

- Primary key.

  If you have not defined the primary key using the column parameters, then you can do this using additional table parameters by adding the entry `PRIMARY KEY (<column_1>, <column_n>)` after enumerating the columns:

  ```sql
  CREATE TABLE Users (
      id INT,
      name VARCHAR(255) NOT NULL,
      age INT NOT NULL DEFAULT 18,
      PRIMARY KEY (id)
  );
  ```

- Foreign keys.

  Suppose we want to store data about the company our users work for. Let's create a small table `Companies` in which we will store a unique identifier and the name of the company:

  ```sql
  CREATE TABLE Companies (
      id INT,
      name VARCHAR(255) NOT NULL,
      PRIMARY KEY (id)
  );
  ```

  Next, you need to add the `company` field to the `Users` table – the place of work of our user, which will refer to the entry in the `Companies' table. The full query for creating a table will look like this:

  ```sql
  CREATE TABLE Users (
      id INT,
      name VARCHAR(255) NOT NULL,
      age INT NOT NULL DEFAULT 18,
      company INT,
      PRIMARY KEY (id)
  );
  ```

  A foreign key is used to ensure that the company column contains an identifier that exists in the `Companies` table when new entries are added to the `Users` table.
  It has the following syntax:

  ```sql
  FOREIGN KEY (<column_1>, <column_n>)
  REFERENCES <external_table> (<external_table_column_1>, <external_table_column_n>)
  [ON DELETE reference_option]
  [ON UPDATE reference_option]
  ```

  The full query for creating a table with a foreign key will be as follows:

  ```sql
  CREATE TABLE Users (
      id INT,
      name VARCHAR(255) NOT NULL,
      age INT NOT NULL DEFAULT 18,
      company INT,
      PRIMARY KEY (id),
      FOREIGN KEY (company) REFERENCES Companies (id)
  );
  ```

  If you have foreign keys, you can determine the behavior of the current record when changing or deleting the record to which it refers.

  ```sql
  CREATE TABLE Users (
      id INT,
      name VARCHAR(255) NOT NULL,
      age INT NOT NULL DEFAULT 18,
      company INT,
      PRIMARY KEY (id),
      FOREIGN KEY (company) REFERENCES Companies (id)
      ON DELETE RESTRICT ON UPDATE CASCADE
  );
  ```

  `ON DELETE RESTRICT` means that if you try to delete a company that has data in the Users table, the database won't let you do this:

  ```sql
  Cannot delete or update a parent row: a foreign key constraint fails
  ```

  If `ON DELETE CASCADE` was specified, then when deleting the company, all users would be deleted, referring to this company.

  There is one more option - `ON DELETE SET NULL`. When used, the database will write `NULL` as the value of the company field for all users who worked at the remote company.

  `ON UPDATE CASCADE` means that if a company changes its ID, then all Users will get a new ID in the company field.

## Deleting a table

Deleting a table is done using the `DROP TABLE` operator.

```sql
DROP TABLE [IF EXIST] table_name;
```
