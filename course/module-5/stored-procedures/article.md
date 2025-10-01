---
meta:
    title: 'Stored Procedures in SQL'
    description: 'Creating and using stored procedures in SQL. Syntax, parameters, conditional logic, loops, and practical examples.'
---

# Stored Procedures in SQL

Stored procedures are program blocks that execute a specific sequence of actions in a database.

<MySQLOnly>

Unlike functions, procedures can modify data, perform complex business logic, and don't necessarily return a value.

</MySQLOnly>

<PostgreSQLOnly>

Unlike functions, procedures can modify data, perform complex business logic, but cannot return values.

</PostgreSQLOnly>

## General Structure of a Stored Procedure

<MySQLOnly>

```sql
CREATE PROCEDURE procedure_name(parameter1 TYPE, parameter2 TYPE, ...)
BEGIN
    -- procedure logic
END;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql
CREATE OR REPLACE PROCEDURE procedure_name(parameter1 TYPE, parameter2 TYPE, ...)
LANGUAGE plpgsql
AS $$
BEGIN
    -- procedure logic
END;
$$;
```

`LANGUAGE plpgsql` — specifies that the procedure is written in **PL/pgSQL** (PostgreSQL's procedural language).

`AS $$ ... $$` — **dollar quoting**, a special way to delimit the procedure body. Allows you to avoid escaping characters inside the procedure.

</PostgreSQLOnly>

## Simple Procedure Example

Let's create a procedure to update student information:

<MySQLOnly>

```sql-executable-Schedule
-- Create procedure
CREATE PROCEDURE update_student_info(
    IN student_id INT,
    IN new_first_name VARCHAR(50),
    IN new_last_name VARCHAR(50)
)
BEGIN
    UPDATE Student
    SET first_name = new_first_name,
        last_name = new_last_name
    WHERE id = student_id;
END;

-- Call procedure
CALL update_student_info(1, 'Alexander', 'Smirnov');

-- Check the result
SELECT * FROM Student WHERE id = 1;
```

</MySQLOnly>

<PostgreSQLOnly>

```sql-executable-Schedule
-- Create procedure
CREATE OR REPLACE PROCEDURE update_student_info(
    student_id INT,
    new_first_name VARCHAR(50),
    new_last_name VARCHAR(50)
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE Student
    SET first_name = new_first_name,
        last_name = new_last_name
    WHERE id = student_id;
END;
$$;

-- Call procedure
CALL update_student_info(1, 'Alexander', 'Smirnov');

-- Check the result
SELECT * FROM Student WHERE id = 1;
```

</PostgreSQLOnly>

This procedure takes a student ID and new data, then updates the corresponding record in the `Student` table.

## Types of Procedure Parameters

<MySQLOnly>

MySQL procedures support three types of parameters that can be passed to a stored procedure:

-   **IN** — input parameters (default)
-   **OUT** — output parameters for returning values
-   **INOUT** — parameters that can be both input and output

### Input Parameters (IN)

Input parameters pass data into the procedure. This is the most common type of parameter:

```sql-executable-Schedule
CREATE PROCEDURE add_subject(
    IN subject_id INT,
    IN subject_name VARCHAR(100)
)
BEGIN
    INSERT INTO Subject (id, name)
    VALUES (subject_id, subject_name);
END;

-- Call with input parameters
CALL add_subject(15, 'Mathematics');
```

### Output Parameters (OUT)

Output parameters allow procedures to return values:

```sql-executable-Schedule
CREATE PROCEDURE get_student_info(
    IN student_id INT,
    OUT student_name VARCHAR(100),
    OUT student_age INT
)
BEGIN
    SELECT
        CONCAT(first_name, ' ', last_name),
        TIMESTAMPDIFF(YEAR, birthday, CURDATE())
    INTO student_name, student_age
    FROM Student
    WHERE id = student_id;
END;

-- Call procedure with output parameters
CALL get_student_info(1, @name, @age);
SELECT @name AS student_name, @age AS student_age;
```

### Input-Output Parameters (INOUT)

INOUT parameters can accept a value and return a modified value:

```sql-executable-Schedule
CREATE PROCEDURE calculate_discount(
    INOUT price DECIMAL(10,2),
    IN discount_percent INT
)
BEGIN
    SET price = price - (price * discount_percent / 100);
END;

-- Using INOUT parameter
SET @original_price = 1000.00;
CALL calculate_discount(@original_price, 15);
SELECT @original_price AS discounted_price;
```

</MySQLOnly>



<MySQLOnly>
### Example of Three Parameter Types

![Examples of parameter usage in stored procedures](https://sql-academy.org/static/guidePage/stored-procedures/params-description.jpg 'Examples of parameter usage in stored procedures')

</MySQLOnly>

### Key Differences Between Parameter Types

<MySQLOnly>

| Parameter Type | Direction     | Usage                        |
| -------------- | ------------- | ---------------------------- |
| **IN**         | Incoming      | Pass data to procedure       |
| **OUT**        | Outgoing      | Return result from procedure |
| **INOUT**      | Bidirectional | Modify passed value          |

> **Important:** OUT and INOUT parameters in MySQL require using session variables (e.g., `@variable_name`) when calling the procedure.

</MySQLOnly>

<PostgreSQLOnly>

PostgreSQL procedures focus on performing actions rather than returning values. For returning values, it's better to use functions.

> **Tip:** If you need to return a value from PostgreSQL, consider using a function instead of a procedure.

</PostgreSQLOnly>

## Managing Stored Procedures

-   **Viewing Existing Procedures**

    <MySQLOnly>

    ```sql
    SHOW PROCEDURE STATUS WHERE Db = 'your_database_name';
    ```

    </MySQLOnly>

    <PostgreSQLOnly>

    ```sql
    SELECT routine_name, routine_type
    FROM information_schema.routines
    WHERE routine_type = 'PROCEDURE' AND routine_schema = 'public';
    ```

    </PostgreSQLOnly>

-   **Dropping a Procedure**

    <MySQLOnly>

    ```sql
    DROP PROCEDURE IF EXISTS add_student;
    ```

    </MySQLOnly>

    <PostgreSQLOnly>

    ```sql
    DROP PROCEDURE IF EXISTS add_student(VARCHAR, VARCHAR, DATE);
    ```

    </PostgreSQLOnly>

-   **Modifying a Procedure**

    <MySQLOnly>

    To modify a procedure in MySQL, you need to drop the old version first, then create a new one:

    ```sql
    DROP PROCEDURE IF EXISTS add_student;
    -- Create new version of the procedure
    CREATE PROCEDURE add_student(...) ...
    ```

    </MySQLOnly>

    <PostgreSQLOnly>

    In PostgreSQL, you can use `CREATE OR REPLACE PROCEDURE`:

    ```sql
    CREATE OR REPLACE PROCEDURE add_student(
        student_first_name VARCHAR(50),
        student_last_name VARCHAR(50),
        student_birthday DATE
    )
    -- new implementation
    ```

    </PostgreSQLOnly>

Stored procedures are a powerful tool for implementing complex business logic directly in the database. They ensure logic centralization, improve performance, and guarantee data integrity! 🚀
