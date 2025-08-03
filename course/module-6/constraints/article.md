---
meta:
  title: "SQL Constraints: MySQL and PostgreSQL"
  description: "A comprehensive guide to SQL constraints in MySQL and PostgreSQL, explaining how they ensure data correctness and integrity in database tables. Learn about different types of constraints including PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK, and DEFAULT."
---

# Column Constraints in SQL

Constraints are rules applied to data in a table to maintain accuracy and reliability. They play a crucial role in ensuring data integrity and compliance with business rules. 🔒

When you create or modify a table structure, you can define various constraints that prevent adding, modifying, or deleting data that violates established rules. This helps avoid undesirable situations such as:

- Having multiple users with identical identifiers
- References to non-existent records in other tables
- Missing required data
- Entering incorrect values (e.g., negative age or future birth date)

## Main Types of SQL Constraints ✨

SQL has the following main types of constraints:

1. **PRIMARY KEY** — unique identifier for a record in a table
2. **FOREIGN KEY** — ensures referential integrity between tables
3. **UNIQUE** — guarantees uniqueness of values in a column or group of columns
4. **NOT NULL** — prohibits NULL values in a column
5. **CHECK** — verifies that data meets a specified condition
6. **DEFAULT** — sets a default value for a column

Let's look at each type in more detail.

## PRIMARY KEY

A primary key is a column or combination of columns that uniquely identifies each row in a table. It cannot contain NULL values and must be unique. A table can have only one primary key.

<MySQLOnly>

```sql
CREATE TABLE Users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
);
```

Alternative syntax using a separate constraint definition:

```sql
CREATE TABLE Users (
    id INT AUTO_INCREMENT,
    username VARCHAR(50),
    email VARCHAR(100),
    CONSTRAINT pk_users PRIMARY KEY (id)
);
```

When attempting to add a record with an already existing primary key or with a NULL value instead of a key, the DBMS will return an error:

```sql
Error(1062) 23000: "Duplicate entry '1' for key 'users.PRIMARY'"
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
CREATE TABLE Users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
);
```

Alternative syntax using a separate constraint definition:

```sql
CREATE TABLE Users (
    id SERIAL,
    username VARCHAR(50),
    email VARCHAR(100),
    CONSTRAINT pk_users PRIMARY KEY (id)
);
```

When attempting to add a record with an already existing primary key or with a NULL value instead of a key, the DBMS will return an error:

```sql
ERROR: duplicate key value violates unique constraint "users_pkey"
DETAIL: Key (id)=(1) already exists.
```

</PostgreSQLOnly>

## FOREIGN KEY

A foreign key is a column or group of columns in one table that references the primary key of another table. It ensures referential integrity of data, guaranteeing that values in the foreign key column correspond to values from the primary key column of the related table.

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    order_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(id)
);
```

Thanks to the FOREIGN KEY constraint:

- You cannot add an order for a non-existent user
- You cannot delete a user who has orders (unless special CASCADE options are specified)

You can also specify actions to be performed when related data is deleted or updated:

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    order_date DATE,
    FOREIGN KEY (user_id) REFERENCES Users(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

Possible options for `ON DELETE` and `ON UPDATE`:

- `CASCADE` — automatically deletes or updates related records
- `SET NULL` — sets NULL for the foreign key
- `SET DEFAULT` — sets the default value
- `RESTRICT` — prevents deletion or update (used by default)
- `NO ACTION` — similar to RESTRICT in most DBMSs

## UNIQUE

The UNIQUE constraint ensures that all values in a column or group of columns are unique. Unlike PRIMARY KEY, it allows NULL values (usually only one NULL value, since NULL != NULL).

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(100) UNIQUE
);
```

This will prevent creating multiple users with the same username or email address.

## NOT NULL

The NOT NULL constraint ensures that a column cannot contain NULL values. This is useful for mandatory fields without which a record would not make sense.

```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    bio TEXT
);
```

In this example, the `username` and `email` fields are required, while `bio` can be empty.

## CHECK

<MySQLOnly>

The CHECK constraint allows you to define a condition that values in a column must satisfy. This helps enforce business rules and prevent incorrect data entry.

**Note:** CHECK constraints are fully supported in MySQL starting from version 8.0.16. In earlier versions, they were accepted syntactically but not enforced.

```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) CHECK (price > 0),
    quantity INT CHECK (quantity >= 0)
);
```

A more complex example with a named constraint:

```sql
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birth_date DATE NOT NULL,
    hire_date DATE NOT NULL,
    CONSTRAINT chk_dates CHECK (hire_date > birth_date)
);
```

</MySQLOnly>

<PostgreSQLOnly>

The CHECK constraint allows you to define a condition that values in a column must satisfy. This helps enforce business rules and prevent incorrect data entry.

PostgreSQL fully supports CHECK constraints and actively enforces them.

```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10, 2) CHECK (price > 0),
    quantity INT CHECK (quantity >= 0)
);
```

A more complex example with a named constraint:

```sql
CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birth_date DATE NOT NULL,
    hire_date DATE NOT NULL,
    CONSTRAINT chk_dates CHECK (hire_date > birth_date)
);
```

PostgreSQL also supports complex CHECK constraints with subqueries and functions:

```sql
CREATE TABLE Users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(100) CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'),
    age INT CHECK (age >= 0 AND age <= 150)
);
```

</PostgreSQLOnly>

## DEFAULT

The DEFAULT constraint sets a value that will be used if no value is specified for this column when adding a new record.

<MySQLOnly>

```sql
CREATE TABLE Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    order_date DATE DEFAULT (CURRENT_DATE),
    status VARCHAR(20) DEFAULT 'Pending',
    FOREIGN KEY (user_id) REFERENCES Users(id)
);
```

In this example, if no order date is specified, the current date will be used, and the status will default to "Pending".

</MySQLOnly>

<PostgreSQLOnly>

```sql
CREATE TABLE Orders (
    order_id SERIAL PRIMARY KEY,
    user_id INT,
    order_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'Pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(id)
);
```

In this example, if no order date is specified, the current date will be used, and the status will default to "Pending".

PostgreSQL also supports more complex DEFAULT values, including function calls:

```sql
CREATE TABLE Users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50),
    created_at TIMESTAMP DEFAULT NOW(),
    uuid_field UUID DEFAULT gen_random_uuid()
);
```

</PostgreSQLOnly>

## Adding and Removing Constraints

<MySQLOnly>

Constraints can be added not only when creating a table but also when modifying it:

```sql
-- Adding a PRIMARY KEY constraint
ALTER TABLE Users
ADD PRIMARY KEY (id);

-- Adding a FOREIGN KEY constraint
ALTER TABLE Orders
ADD CONSTRAINT fk_user FOREIGN KEY (user_id) REFERENCES Users(id);

-- Adding a UNIQUE constraint
ALTER TABLE Users
ADD CONSTRAINT uq_email UNIQUE (email);

-- Adding a CHECK constraint
ALTER TABLE Products
ADD CONSTRAINT chk_price CHECK (price > 0);

-- Adding a NOT NULL constraint
ALTER TABLE Users
MODIFY username VARCHAR(50) NOT NULL;

-- Adding a default value
ALTER TABLE Orders
ALTER COLUMN status SET DEFAULT 'Pending';
```

Constraints can also be removed using the ALTER TABLE command:

```sql
-- Removing a PRIMARY KEY constraint
ALTER TABLE Users
DROP PRIMARY KEY;

-- Removing a FOREIGN KEY constraint
ALTER TABLE Orders
DROP FOREIGN KEY fk_user;

-- Removing a UNIQUE constraint
ALTER TABLE Users
DROP INDEX uq_email;

-- Removing a CHECK constraint
ALTER TABLE Products
DROP CHECK chk_price;

-- Removing a NOT NULL constraint
ALTER TABLE Users
MODIFY username VARCHAR(50) NULL;

-- Removing a default value
ALTER TABLE Orders
ALTER COLUMN status DROP DEFAULT;
```

</MySQLOnly>

<PostgreSQLOnly>

Constraints can be added not only when creating a table but also when modifying it:

```sql
-- Adding a PRIMARY KEY constraint
ALTER TABLE Users
ADD PRIMARY KEY (id);

-- Adding a FOREIGN KEY constraint
ALTER TABLE Orders
ADD CONSTRAINT fk_user FOREIGN KEY (user_id) REFERENCES Users(id);

-- Adding a UNIQUE constraint
ALTER TABLE Users
ADD CONSTRAINT uq_email UNIQUE (email);

-- Adding a CHECK constraint
ALTER TABLE Products
ADD CONSTRAINT chk_price CHECK (price > 0);

-- Adding a NOT NULL constraint
ALTER TABLE Users
ALTER COLUMN username SET NOT NULL;

-- Adding a default value
ALTER TABLE Orders
ALTER COLUMN status SET DEFAULT 'Pending';
```

Constraints can also be removed using the ALTER TABLE command:

```sql
-- Removing a PRIMARY KEY constraint
ALTER TABLE Users
DROP CONSTRAINT users_pkey;

-- Removing a FOREIGN KEY constraint
ALTER TABLE Orders
DROP CONSTRAINT fk_user;

-- Removing a UNIQUE constraint
ALTER TABLE Users
DROP CONSTRAINT uq_email;

-- Removing a CHECK constraint
ALTER TABLE Products
DROP CONSTRAINT chk_price;

-- Removing a NOT NULL constraint
ALTER TABLE Users
ALTER COLUMN username DROP NOT NULL;

-- Removing a default value
ALTER TABLE Orders
ALTER COLUMN status DROP DEFAULT;
```

</PostgreSQLOnly>

## Best Practices for Using Constraints 🚀

When designing a database, follow these recommendations:

1. **Always define a primary key** for each table to uniquely identify each record.

2. **Use foreign keys** to ensure referential integrity between related tables.

3. **Apply the NOT NULL constraint** for columns that must contain values.

4. **Use the UNIQUE constraint** for columns that must contain unique values (e.g., email, phone number).

5. **Add CHECK constraints** for columns whose values must comply with certain business rules.

6. **Set default values** for columns that frequently take the same value.

7. **Specify names for constraints** (e.g., `CONSTRAINT pk_users PRIMARY KEY (id)`) to facilitate their identification and management.

## Test Your Knowledge About SQL Constraints:

Which of the following SQL constraints CANNOT contain NULL values?
